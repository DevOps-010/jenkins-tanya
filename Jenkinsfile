pipeline {
    agent any
    triggers {
        cron('H 3 * * *')  // Trigger the build daily at 3 AM
    }
    stages {
        stage('Checkout & Build') {
            steps {
                echo 'Checking out the repository and building the project...'
                // Add your actual build command here
            }
        }
        stage('Test') {
            steps {
                echo 'Running the full suite of tests...'
                // Add your actual test command here
            }
        }
        stage('Archive') {
            steps {
                echo 'Archiving build and test results...'
                // Add archiving commands here if needed
            }
        }
        stage('Report') {
            steps {
                echo 'Sending email with build result summary...'
                // Add email sending command here
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}