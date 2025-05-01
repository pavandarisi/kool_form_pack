pipeline {
    agent {
        label 'sai' // Replace with your actual Jenkins agent label
    }

    environment {
        REPO_URL = 'https://github.com/pavandarisi/kool_form_pack.git'
        APP_DIR = '/home/ubuntu/'         // local directory to clone into
        DEPLOY_PATH = '/var/www/html'   // Apache default root
    }

    stages {
        stage('Clone Repository') {
            steps {
                sh 'rm -rf $APP_DIR'
                sh 'git clone $REPO_URL $APP_DIR'
            }
        }

        stage('Deploy to Apache') {
            steps {
                sh '''
                sudo rm -rf $DEPLOY_PATH/*
                sudo cp -r $APP_DIR/* $DEPLOY_PATH/
                '''
            }
        }

        stage('Restart Apache') {
            steps {
                sh 'sudo systemctl restart apache2'
            }
        }
    }

    post {
        success {
            echo "✅ Static site deployed to Apache. Visit: http://<agent-ip>/"
        }
        failure {
            echo "❌ Deployment failed. Check pipeline logs."
        }
    }
}
