pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ajeetht25-hub/Bike-website.git'
            }
        }

        stage('Validate Files') {
            steps {
                sh '''
                    test -f Dockerfile
                    test -f index.html
                    echo "Website files found."
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t bike-website:${BUILD_NUMBER} .
                    docker tag bike-website:${BUILD_NUMBER} bike-website:latest
                '''
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                    docker rm -f bike-website || true

                    docker run -d \
                        --name bike-website \
                        --restart unless-stopped \
                        -p 8081:80 \
                        bike-website:${BUILD_NUMBER}

                    sleep 3
                    docker ps --filter name=bike-website
                    curl --fail http://localhost:8081/
                '''
            }
        }
    }

    post {
        success {
            echo 'Bike website built and deployed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }
    }
}
