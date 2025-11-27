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

        stage('Build Backend (Maven WAR)') {
            steps {
                dir('backend') {
                    bat 'mvn -B -DskipTests clean package'
                }
            }
        }

        stage('Build Frontend (npm build)') {
            steps {
                dir('frontend') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                bat '''
REM Create temp deploy folder
IF NOT EXIST "%TEMP%\\deploy" mkdir "%TEMP%\\deploy"

REM Copy generated WAR file
copy backend\\target\\*.war "%TEMP%\\deploy\\blood.war" /Y

REM Deploy via Tomcat Manager API
curl -u admin:admin ^
     -T "%TEMP%\\deploy\\blood.war" ^
     "http://localhost:8080/manager/text/deploy?path=/blood&update=true"
'''
            }
        }
    }
}
