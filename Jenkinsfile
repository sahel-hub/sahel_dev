pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Code is already present locally or mounted.'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("devops_project_image")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    dockerImage.run("-p 8080:80")
                }
            }
            
        }
        stage('Verify Deployment') {
            steps {
                echo '🔍 Verifying deployment...'
                bat 'curl http://localhost:8080'
            }
        }
    }
    }
}
