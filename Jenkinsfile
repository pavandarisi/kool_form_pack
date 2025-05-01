pipeline {
    agent {
        label 'sai' // Your Jenkins agent label
    }

    environment {
        REPO_URL = 'https://github.com/pavandarisi/kool_form_pack.git'
        APP_DIR = 'kool_form_pack'
        DEPLOY_PATH = '/var/www/html'
    }

    stages {
        stage('Check Workspace Access') {
            steps {
                dir("${WORKSPACE}") {
                    sh '''
                    echo "🔍 Checking Jenkins workspace directory permissions..."
                    echo "WORKSPACE = $WORKSPACE"
                    mkdir -p $WORKSPACE/test
                    touch $WORKSPACE/test/testfile.txt
                    ls -la $WORKSPACE/test
                    '''
                }
            }
        }

        stage('Clone Repository') {
            steps {
                dir("${WORKSPACE}") {
                    sh 'rm -rf $APP_DIR'
                    sh 'git clone $REPO_URL $APP_DIR'
                }
            }
        }

        stage('Deploy to Apache') {
            steps {
                dir("${WORKSPACE}") {
                    sh '''
                    sudo rm -rf $DEPLOY_PATH/*
                    sudo cp -r $APP_DIR/* $DEPLOY_PATH/
                    '''
                }
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
