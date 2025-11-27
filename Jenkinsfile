pipeline {
    agent any

    environment {
        // Tomcat credentials stored in Jenkins Credentials (replace IDs accordingly)
        TOMCAT_USER = credentials('tomcat-username')  // e.g., 'admin'
        TOMCAT_PASS = credentials('tomcat-password')  // e.g., 'password'
        TOMCAT_URL  = "http://localhost:7777/manager/text"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'main', url: 'https://github.com/ganesh-160905/blood-backend.git'
            }
        }

        stage('Build Backend WAR') {
            steps {
                echo 'Building backend WAR...'
                // On Windows, use bat instead of sh
                bat 'mvn clean package'
            }
        }

        stage('Archive WAR') {
            steps {
                echo 'Archiving WAR...'
                archiveArtifacts artifacts: 'target/blood-backend.war', fingerprint: true
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying WAR to Tomcat...'
                // Deploy WAR using Tomcat manager
                bat """
                curl --upload-file target\\blood-backend.war \
                ${TOMCAT_URL}/deploy?path=/blood-backend&update=true \
                --user ${TOMCAT_USER}:${TOMCAT_PASS}
                """
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment completed successfully!'
        }
        failure {
            echo 'Build or Deployment failed!'
        }
    }
}
