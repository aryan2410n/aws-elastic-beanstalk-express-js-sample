pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root'
        }
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test -- --runInBand'
            }
        }

        stage('Docker Image') {
            steps {
                sh 'docker build -t assessment2-node-app .'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'npm audit --audit-level=high'
            }
        }
    }
}
