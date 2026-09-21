pipeline {
    agent {
        docker {
            image 'assessment2-jenkins-node16'
            args '-u root --add-host docker:172.18.0.2 -e DOCKER_HOST=tcp://docker:2376 -e DOCKER_CERT_PATH=/certs/client -e DOCKER_TLS_VERIFY=1 -v /opt/jenkins-docker-certs:/certs/client:ro'
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
                sh 'npm audit --json > npm-audit.json || true'
                sh 'npm audit --audit-level=high'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'npm-audit.json', fingerprint: true
        }
    }
}

