pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'git-credentials'
        TOMCAT_USER = admin
        TOMCAT_PASS = admin
        TOMCAT_URL = "http://localhost:7777/manager/text"
    }

    stages {
        stage('Checkout Backend') {
            steps {
                dir('backend') {
                    git branch: 'main', url: 'https://github.com/ganesh-160905/blood-backend.git', credentialsId: "${GIT_CREDENTIALS}"
                }
            }
        }

        stage('Build Backend WAR') {
            steps {
                dir('backend') {
                    bat "mvn -B -DskipTests clean package"
                }
            }
        }

        stage('Deploy WAR to Tomcat') {
            steps {
                dir('backend') {
                    withCredentials([string(credentialsId: 'tomcat-credentials-username', variable: 'TOMCAT_USER'), string(credentialsId: 'tomcat-credentials-password', variable: 'TOMCAT_PASS')]) {
                        bat 'curl -u %TOMCAT_USER%:%TOMCAT_PASS% --upload-file target\\blood-backend.war "%TOMCAT_URL%/deploy?path=/blood-backend&update=true"'
                    }
                }
            }
        }

        stage('Checkout Frontend') {
            steps {
                dir('frontend') {
                    git branch: 'main', url: 'https://github.com/ganesh-160905/blood-frontend.git', credentialsId: "${GIT_CREDENTIALS}"
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    bat "npm install"
                    bat "npm run build"
                }
            }
        }

        stage('Post Build Summary') {
            steps {
                echo "Backend + Frontend pipeline completed successfully!"
            }
        }
    }

    post {
        failure {
            echo "Build or Deployment failed!"
        }
        success {
            echo "Pipeline finished successfully!"
        }
    }
}
