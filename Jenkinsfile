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
                bat 'docker-compose build'
            }
        }

        stage('Run Services') {
            steps {
                bat 'docker-compose up -d'
            }
        }

        stage('Run Backend Tests') {
            steps {
                // Replace `backend` with the actual container name if different
                bat 'docker-compose exec backend pytest'
            }
        }

        stage('Run Frontend Tests') {
            steps {
                // Replace `frontend` with the actual container name if different
                bat 'docker-compose exec frontend npm test'
            }
        }

        stage('Stop Services') {
            steps {
                bat 'docker-compose down'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            bat 'docker-compose down --volumes --remove-orphans'
        }
    }
}
