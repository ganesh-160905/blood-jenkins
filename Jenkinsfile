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
                // update admin:password to your Tomcat manager credentials
                bat '''
if not exist "%TEMP%\\deploy" mkdir "%TEMP%\\deploy"
copy backend\\target\\*.war "%TEMP%\\deploy\\app.war" /Y
curl -u admin:admin -T "%TEMP%\\deploy\\app.war" "http://localhost:8080/manager/text/deploy?path=/blood&update=true"
'''
            }
        }
    }
}
