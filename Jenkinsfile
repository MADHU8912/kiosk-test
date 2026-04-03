pipeline {
    agent any

    triggers {
        cron('H/5 * * * *')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Getting code from GitHub...'
            }
        }

        stage('Check Files') {
            steps {
                bat 'dir'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build --no-cache -t kiosk-test-app .'
            }
        }

        stage('Remove Old Container') {
            steps {
                bat 'docker rm -f kiosk-test-container || exit /b 0'
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker run -d -p 8095:80 --name kiosk-test-container kiosk-test-app'
            }
        }

        stage('Create Build Log') {
            steps {
                bat 'echo Build completed successfully > build-log.txt'
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'build-log.txt', fingerprint: true
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}