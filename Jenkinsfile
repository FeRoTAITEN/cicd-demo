pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t my-nginx-app:v1 .
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    docker images
                    docker image inspect my-nginx-app:v1
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker rm -f my-nginx-container || true
                    docker run -d \
                        --name my-nginx-container \
                        -p 8081:80 \
                        my-nginx-app:v1
                '''
            }
        }

       stage('Verify') {
    steps {
        sh '''
            docker exec my-nginx-container wget -qO- http://localhost
        '''
    }
}

    }

    post {

        success {
            echo '✅ Pipeline completed successfully.'
        }

        failure {
            echo '❌ Pipeline failed.'
        }

        always {
            echo 'Pipeline finished.'
        }

    }
}
