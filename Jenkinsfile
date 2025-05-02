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

        stage('Start Services') {
            steps {
                bat 'docker-compose up -d'
            }
        }

        stage('Run Backend Tests') {
            steps {
                // Run pytest in backend
                bat 'docker-compose exec -T backend pytest'
            }
        }

        stage('Run Frontend Tests') {
            steps {
                // Run npm test in frontend-dev (dev container)
                bat 'docker-compose exec -T frontend-dev npm test'
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
            echo 'Cleaning up resources...'
            bat 'docker-compose down --volumes --remove-orphans'
        }
    }
}
