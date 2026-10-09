pipeline {
    agent any

    stages {
    stage('Checkout Code') {
        steps {
            git branch: 'main',
                url: 'https://github.com/ajeetht25-hub/Bike-website.git'
        }
    }

    stage('Check Files') {
        steps {
            sh '''
                pwd
                ls -la
                test -f Dockerfile
                test -f index.html
                echo "Files checked successfully"
            '''
        }
    }

    stage('Build Docker Image') {
        steps {
            sh 'docker build -t bike-website:latest .'
        }
    }

    stage('Deploy Website') {
        steps {
            sh '''
                docker rm -f bike-website || true
                docker run -d --name bike-website \
                    --restart unless-stopped \
                    -p 8081:80 bike-website:latest
                sleep 3
                curl --fail http://localhost:8081/
            '''
        }
    }
}
    
