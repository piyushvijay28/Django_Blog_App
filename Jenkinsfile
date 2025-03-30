pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/piyushvijay28/Django_Blog_App.git'  // GitHub repository URL
        IMAGE_NAME = 'blog-image-1'  // Name of the Docker image
        CONTAINER_NAME = 'blog-container-1'  // Name of the Docker container
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Pull code from GitHub repository
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh 'docker build -t ${IMAGE_NAME} .'  // Build using existing Dockerfile
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove any previously running container with the same name
                    echo "Stopping and removing previous container (if any)..."
                    sh 'docker stop ${CONTAINER_NAME} || true'
                    sh 'docker rm ${CONTAINER_NAME} || true'

                    // Run the Docker container from the image
                    echo "Running Docker container..."
                    sh 'docker run -d --name ${CONTAINER_NAME} -p 8081:8000 ${IMAGE_NAME}'
                }
            }
        }

        stage('Verify Docker Container') {
            steps {
                script {
                    echo "Verifying Docker container..."
                    sh 'docker ps -a'  // Lists all containers, including stopped ones
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution finished."
        }

        success {
            echo "Pipeline ran successfully!"
        }

        failure {
            echo "Pipeline failed. Please check the logs for more information."
        }
    }
}
