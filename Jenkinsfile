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
                echo "Checking project files..."
                pwd
                ls -la
                test -f Dockerfile
                test -f index.html
                echo "Files checked successfully"
            '''
        }
    }

    stage('Pull Nginx Image') {
        steps {
            retry(3) {
                sh 'docker pull nginx:latest'
            }
        }
    }

    stage('Build Docker Image') {
        steps {
            sh '''
                docker build --pull=false -t bike-website:latest .
            '''
        }
    }

    stage('Deploy Website') {
        steps {
            sh '''
                docker rm -f bike-website || true

                docker run -d \
                    --name bike-website \
                    -p 8081:80 \
                    --restart unless-stopped \
                    bike-website:latest

                echo "Website deployed successfully"
            '''
        }
    }
}

post {
    success {
        echo 'Pipeline completed successfully!'
    }

    failure {
        echo 'Pipeline failed. Check the Jenkins console output.'
    }

    always {
        echo 'Pipeline execution finished.'
    }
}

}
