pipeline {
    agent {
        label 'sai' // Replace with your actual Jenkins agent label
    }

    environment {
        REPO_URL = 'https://github.com/pavandarisi/kool_form_pack.git'
        DEPLOY_PATH = '/var/www/html' // Apache's default root
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "📥 Cloning repo..."
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Deploy to Apache') {
            steps {
                echo "🚀 Deploying static files to Apache root..."
                sh '''
                sudo rm -rf ${DEPLOY_PATH}/*
                sudo cp -r * ${DEPLOY_PATH}/
                '''
            }
        }

        stage('Restart Apache') {
            steps {
                echo "🔄 Restarting Apache..."
                sh 'sudo systemctl restart apache2'
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
