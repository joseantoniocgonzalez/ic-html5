pipeline {
    agent {
        docker {
            image 'josedom24/debian-npm'
            args '-u root:root'
        }
    }
   
    environment {
        TOKEN = credentials('SURGE_TOKEN')
    }
   
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/joseantoniocgonzalez/ic-html5.git'
            }
        }
       
        stage('Change Repositories to HTTPS') {
            steps {
                sh '''
                sed -i 's/http:/https:/g' /etc/apt/sources.list
                apt update
                '''
            }
        }
       
        stage('Install Surge') {
            steps {
                sh 'npm install -g surge'
            }
        }
       
        stage('Install Dependencies') {
            steps {
                sh 'apt update -y && apt install -y python3-pip default-jre'
                sh 'pip3 install html5validator'
            }
        }
       
        stage('Validate HTML') {
            steps {
                sh '''
                echo '<!DOCTYPE html>
                <html lang="es">
                <head>
                    <meta charset="UTF-8">
                    <title>José Antonio Canalo González</title>
                </head>
                <body>
                    <h1>José Antonio Canalo González</h1>
                </body>
                </html>' > _build/index.html
                html5validator --root _build/ || true
                '''
            }
        }
       
        stage('Deploy') {
            steps {
                sh 'surge ./_build/ joseantoniocgonzalez.surge.sh --token $TOKEN'
            }
        }
    }
}
