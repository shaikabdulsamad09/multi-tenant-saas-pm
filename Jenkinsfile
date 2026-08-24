pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = 'multi-tenant-saas-pm'
        BACKEND_CONTAINER = 'saas-backend'
        FRONTEND_CONTAINER = 'saas-frontend'
        POSTGRES_CONTAINER = 'saas-postgres'
        BACKEND_PORT = '5000'
        FRONTEND_PORT = '3000'
        DB_PORT = '5432'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out project source from GitHub...'
                checkout scm
            }
        }

        stage('Prepare environment') {
            steps {
                sh 'docker --version'
                sh 'docker compose version'
            }
        }

        stage('Build Docker images') {
            steps {
                sh 'docker compose build --no-cache'
            }
        }

        stage('Deploy application') {
            steps {
                sh 'docker compose down --remove-orphans || true'
                sh 'docker compose up -d --remove-orphans'
            }
        }

        stage('Verify deployment') {
            steps {
                sh 'docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"'
                sh 'curl -fsS http://localhost:${BACKEND_PORT}/api/health'
                sh 'curl -fsSI http://localhost:${FRONTEND_PORT}'
            }
        }
    }

    post {
        success {
            echo 'Multi-tenant SaaS platform deployed successfully via Docker Compose.'
        }
        failure {
            echo 'Pipeline failed. Check the console output and Docker logs.'
        }
    }
}