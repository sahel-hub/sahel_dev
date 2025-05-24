pipeline {
    agent any

    environment {
        IMAGE_NAME = 'devops_project_image'
    }

    stages {
        stage('Check Docker') {
            steps {
                bat 'docker version || echo Docker is not running! Please start Docker.'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        bat "docker build -t %IMAGE_NAME% ."
                    } catch (Exception e) {
                        error("❌ Docker build failed. Is Docker running?\n" + e.getMessage())
                    }
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    try {
                        bat "docker run -d -p 8080:80 --name devops_container %IMAGE_NAME%"
                    } catch (Exception e) {
                        error("❌ Failed to run Docker container.\n" + e.getMessage())
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '🔍 Verifying deployment...'
                bat 'curl http://localhost:8080 || echo Verification failed.'
            }
        }
    }

    post {
        always {
            echo '🧹 Cleaning up Docker container...'
            bat 'docker stop devops_container || echo Nothing to stop.'
            bat 'docker rm devops_container || echo Nothing to remove.'
        }
    }
}
