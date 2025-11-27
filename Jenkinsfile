pipeline {
    agent any

    environment {
        // Tomcat deployment URL
        TOMCAT_URL = "http://localhost:7777/manager/text"
        // Jenkins credential ID for Tomcat (username + password)
        TOMCAT_CREDS = credentials('tomcat-credentials')
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out code..."
                git branch: 'main', url: 'https://github.com/ganesh-160905/blood-backend.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building Spring Boot WAR..."
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo "Deploying WAR to Tomcat..."
                // Using curl to deploy the WAR to Tomcat
                bat """
                curl --fail --upload-file target\\blood-backend.war \\
                     "${TOMCAT_URL}/deploy?path=/blood-backend&update=true" \\
                     --user ${TOMCAT_CREDS_USR}:${TOMCAT_CREDS_PSW}
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Build or Deployment failed!'
        }
    }
}
