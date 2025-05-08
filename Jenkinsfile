pipeline {
    agent any

    environment {
        DEPLOY_USER = 'username'
        DEPLOY_HOST = 'server'
        DEPLOY_PATH = '/path/to/deployment/folder'
        SITE_URL = 'http://server_link.com'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/your-repo.git'
            }
        }

        stage('Install Linters') {
            steps {
                sh 'npm install -g htmlhint csslint'
            }
        }

        stage('Run HTMLHint') {
            steps {
                sh 'htmlhint **/*.html'
            }
        }

        stage('Run CSSLint') {
            steps {
                sh 'csslint **/*.css || true' // Prevent failure on CSS warnings
            }
        }

        stage('Package Files') {
            steps {
                sh 'zip -r website.zip . -x "*.git*" "node_modules/*"'
            }
        }

        stage('Deploy to Server') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'jenkins-ssh-key', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                    scp -i $SSH_KEY website.zip $DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH
                    '''
                }
            }
        }

        stage('Notify') {
            steps {
                mail to: 'to@example.com',
                     subject: 'Static Site Deployment - SUCCESS',
                     body: "The website has been deployed successfully. Visit: ${env.SITE_URL}"
            }
        }
    }

    post {
        failure {
            mail to: 'to@example.com',
                 subject: 'Static Site Deployment - FAILED',
                 body: "Deployment failed. Please check Jenkins logs for details."
        }
    }
}
