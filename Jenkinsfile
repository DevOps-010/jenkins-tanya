pipeline {
    agent any

    environment {
        DEPLOY_USER = 'username'
        DEPLOY_HOST = 'server'
        DEPLOY_PATH = '/path/to/deployment/folder'
        SITE_URL = 'http://server_link.com'
        SSH_KEY = credentials('SSH_PRIVATE_KEY') // Assumes this is stored in Jenkins Credentials
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/DevOps-010/jenkins-tanya'
            }
        }

        stage('Install Linters') {
            steps {
                sh 'npm install -g htmlhint csslint'
            }
        }

        stage('Run HTMLHint') {
            steps {
                sh 'htmlhint **/*.html || true'
            }
        }

        stage('Run CSSLint') {
            steps {
                sh 'csslint **/*.css || true'
            }
        }

        stage('Package Files') {
            steps {
                sh 'zip -r website.zip .'
            }
        }

        stage('Deploy to Server') {
            steps {
                sh '''
                echo "$SSH_KEY" > id_rsa
                chmod 600 id_rsa
                scp -i id_rsa -o StrictHostKeyChecking=no website.zip $DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH
                rm id_rsa
                '''
            }
        }

        stage('Notify') {
            steps {
                echo "Deployment successful! Website available at $SITE_URL"
                // Optional: Email notification
                // mail to: 'to@example.com',
                //      subject: "Deployment Successful",
                //      body: "The website has been deployed to $SITE_URL"
            }
        }
    }

    post {
        failure {
            echo 'Build failed!'
            // Optional: Email notification on failure
            // mail to: 'to@example.com',
            //      subject: "Deployment Failed",
            //      body: "The deployment failed. Please check Jenkins logs."
        }
    }
}

