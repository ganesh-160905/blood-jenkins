pipeline {
    agent any

    environment {
        TOMCAT_USER = "admin"
        TOMCAT_PASS = "admin"
        TOMCAT_PORT = "7777"
        TOMCAT_URL  = "http://localhost:7777/manager/text"
    }

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

        stage('Deploy Backend to Tomcat (/back1)') {
            steps {
                bat '''
IF NOT EXIST "%TEMP%\\deploy" mkdir "%TEMP%\\deploy"
copy backend\\target\\*.war "%TEMP%\\deploy\\back1.war" /Y

curl -u admin:admin ^
     -T "%TEMP%\\deploy\\back1.war" ^
     "http://localhost:7777/manager/text/deploy?path=/back1&update=true"
'''
            }
        }
    }
}
