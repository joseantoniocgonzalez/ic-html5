pipeline {
    environment {
        TOKEN = credentials('SURGE_TOKEN')  // Token guardado en Jenkins
    }
    agent {
        docker {
            image 'josedom24/debian-npm'  // Imagen con npm preinstalado
            args '-u root:root'          // Ejecutar como root
        }
    }
    stages {
        stage('Clone') {
            steps {
                echo '🔄 Clonando tu repositorio...'
                git branch: 'master', url: 'https://github.com/joseantoniocgonzalez/ic-html5.git'
            }
        }

        stage('Install surge') {
            steps {
                echo '📦 Instalando Surge...'
                sh 'npm install -g surge'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Desplegando en Surge.sh...'
                sh 'surge ./_build/ joseantoniocgonzalez.surge.sh --token $TOKEN'
            }
        }
    }
}
