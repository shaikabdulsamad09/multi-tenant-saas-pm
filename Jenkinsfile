pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning project from GitHub...'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t cloud-based-multi-tenant-saas-project-management-platform .
                '''
            }
        }

        stage('Stop Existing Container') {
            steps {
                bat '''
                docker stop cloud-based-multi-tenant-saas || exit /b 0
                docker rm cloud-based-multi-tenant-saas || exit /b 0
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '''
                docker run -d ^
                --name cloud-based-multi-tenant-saas ^
                -p 3000:3000 ^
                cloud-based-multi-tenant-saas-project-management-platform
                '''
            }
        }

        stage('Verify Container') {
            steps {
                bat '''
                docker ps
                '''
            }
        }
    }

    post {
        success {
            echo 'Project built and deployed successfully!'
        }

        failure {
            echo 'Build or deployment failed.'
        }
    }
}