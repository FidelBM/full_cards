pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/FidelBM/full_cards.git'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Repository with Submodules') {
            steps {
                // Clonar el repositorio e inicializar los submódulos
                sh "git clone --recurse-submodules ${env.GIT_REPO}"
                dir('full_cards') {
                    sh 'git submodule update --init --recursive'
                }
            }
        }

        stage('Stop Running Containers') {
            steps {
                dir('full_cards') {
                    // Detener y eliminar los contenedores actuales
                    sh 'docker-compose down'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                dir('full_cards') {
                    // Construir imágenes Docker
                    sh 'docker-compose build'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                dir('full_cards') {
                    // Subir los contenedores en modo detached
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        success {
            echo 'Application successfully deployed to production.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more information.'
        }
    }
}
