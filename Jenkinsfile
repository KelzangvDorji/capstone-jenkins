pipeline {
    agent any

    environment {
        COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Run Services') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Run Backend Tests') {
            steps {
                // Assuming your backend service is named "backend"
                sh 'docker-compose exec backend pytest'
            }
        }

        stage('Run Frontend Tests') {
            steps {
                // Assuming your frontend service is named "frontend"
                sh 'docker-compose exec frontend npm test'
            }
        }

        stage('Stop Services') {
            steps {
                sh 'docker-compose down'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker-compose down --volumes --remove-orphans'
        }
    }
}
