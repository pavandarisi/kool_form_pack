pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "saipavan16/kool_from_pack"
        TAG = "v1.0.${BUILD_NUMBER}"
        CONTAINER_NAME = "kool_form_app"
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning source code..."
                git branch: 'main', url: 'https://github.com/pavandarisi/kool_form_pack.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${DOCKER_IMAGE}:${TAG} ."
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                echo "Logging into Docker Hub..."
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                sh "docker push ${DOCKER_IMAGE}:${TAG}"
            }
        }
        
        stage('Deploy Container') {
            steps {
                echo "Deploying container..."
                sh """
                docker rm -f ${CONTAINER_NAME} || true
                docker run -d -p 80:80 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:${TAG}
                """
            }
        }
    }
    
    post {
        success {
            echo "✅ Deployment complete. Visit: http://<agent-ip>/"
        }
        failure {
            echo "❌ Deployment failed. Check pipeline logs for details."
        }
    }
}
