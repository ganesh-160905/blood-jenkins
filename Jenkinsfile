pipeline {
    agent any

    environment {
        // Change these based on your Jenkins credentials
        GIT_CREDENTIALS = 'git-credentials'
        TOMCAT_USER = credentials('tomcat-credentials-username')
        TOMCAT_PASS = credentials('tomcat-credentials-password')
        TOMCAT_URL = "http://localhost:7777/manager/text"
    }

    stages {

        stage('Checkout Backend') {
            steps {
                git branch: 'main',
                    credentialsId: "${GIT_CREDENTIALS}",
                    url: 'https://github.com/ganesh-160905/blood-backend.git'
            }
        }

        stage('Build Backend WAR') {
            steps {
                bat "mvn clean package -DskipTests"
            }
        }

        stage('Deploy WAR to Tomcat') {
            steps {
                echo "Deploying WAR to Tomcat..."
                bat """
                    curl -u ${TOMCAT_USER}:${TOMCAT_PASS} ^
                    --upload-file target/blood-backend.war ^
                    "${TOMCAT_URL}/deploy?path=/blood-backend&update=true"
                """
            }
        }

        stage('Checkout Frontend') {
            steps {
                git branch: 'main',
                    credentialsId: "${GIT_CREDENTIALS}",
                    url: 'https://github.com/ganesh-160905/blood-frontend.git'
            }
        }

        stage('Build Frontend') {
            steps {
                bat """
                    npm install
                    npm run build
                """
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
