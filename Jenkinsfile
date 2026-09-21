pipeline {
    agent {
        docker {
            image 'assessment2-jenkins-node16'
            args '-u root -e DOCKER_HOST=tcp://docker:2376 -e DOCKER_CERT_PATH=/certs/client -e DOCKER_TLS_VERIFY=1 -v isec6000-assessment2-jenkins_docker_certs:/certs/client:ro'
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
