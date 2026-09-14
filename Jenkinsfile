pipeline {
    agent any

    tools {
        jdk 'JDK-11'
        maven 'Maven-3.8.7'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Trivy Filesystem Scan') {
            steps {
                sh 'trivy fs --severity HIGH,CRITICAL --no-progress .'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.11.0.3922:sonar'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'nexus-credentials',
                        usernameVariable: 'NEXUS_USER',
                        passwordVariable: 'NEXUS_PASS'
                    )
                ]) {
                    sh '''
                        cat > nexus-settings.xml <<EOF
<settings>
    <servers>
        <server>
            <id>boardgame-snapshots</id>
            <username>${NEXUS_USER}</username>
            <password>${NEXUS_PASS}</password>
        </server>
    </servers>
</settings>
EOF

                        JAR_FILE=$(find target -maxdepth 1 -type f -name "*.jar" ! -name "*sources*" ! -name "*javadoc*" | head -1)

                        echo "Uploading artifact: $JAR_FILE"

                        mvn deploy:deploy-file \
                          -Dfile="$JAR_FILE" \
                          -DpomFile=pom.xml \
                          -DrepositoryId=boardgame-snapshots \
                          -Durl=http://172.31.89.124:8081/repository/boardgame-snapshots/ \
                          -s nexus-settings.xml

                        rm -f nexus-settings.xml
                    '''
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t boardgame:latest .'
            }
        }

        stage('Trivy Image Scan') {
            steps {
                sh 'trivy image --severity HIGH,CRITICAL --no-progress boardgame:latest'
            }
        }

        stage('Push Docker Image to Nexus') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'nexus-credentials',
                        usernameVariable: 'NEXUS_USER',
                        passwordVariable: 'NEXUS_PASS'
                    )
                ]) {
                    sh '''
                        echo "$NEXUS_PASS" | docker login 172.31.89.124:8081 \
                          -u "$NEXUS_USER" \
                          --password-stdin

                        docker tag boardgame:latest \
                          172.31.89.124:8081/boardgame-docker/boardgame:latest

                        docker push \
                          172.31.89.124:8081/boardgame-docker/boardgame:latest
                    '''
                }
            }
        }
    }
}
