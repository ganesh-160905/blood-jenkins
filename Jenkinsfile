pipeline {
    agent any

    stages {
        stage('Clone Backend') {
            steps {
                dir('backend') {
                    git branch: 'main', url: 'https://github.com/ganesh-160905/blood-backend.git'
                }
            }
        }

        stage('Clone Frontend') {
            steps {
                dir('frontend') {
                    git branch: 'main', url: 'https://github.com/ganesh-160905/blood-frontend.git'
                }
            }
        }

        stage('Build Backend (WAR)') {
            steps {
                dir('backend') {
                    bat 'mvn -B -DskipTests clean package'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        stage('Deploy WAR to Tomcat') {
            steps {
                bat """
if not exist "%TEMP%\\deploy" mkdir "%TEMP%\\deploy"

rem Copy WAR file
copy backend\\target\\*.war "%TEMP%\\deploy\\blood.war" /Y

rem Deploy to Tomcat (port 7777)
curl -u admin:admin -T "%TEMP%\\deploy\\blood.war" "http://localhost:7777/manager/text/deploy?path=/blood^&update=true"
"""
            }
        }
    }
}
