pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ajeetht25-hub/Bike-website.git'
            }
        }

        stage('Check Website Files') {
            steps {
                sh '''
                    echo "Checking website files..."

                    if [ ! -f index.html ]; then
                        echo "ERROR: index.html not found in repository root"
                        exit 1
                    fi

                    echo "Website entry point found."
                    find . -maxdepth 2 -type f \
                        ! -path './.git/*' \
                        ! -path './.gitignore' \
                        | sort
                '''
            }
        }

        stage('Validate HTML') {
            steps {
                sh '''
                    if command -v tidy >/dev/null 2>&1; then
                        tidy -q -e index.html || true
                    else
                        echo "HTML Tidy is not installed; skipping HTML lint."
                    fi
                '''
            }
        }

        stage('Archive Website') {
            steps {
                archiveArtifacts artifacts: '**/*',
                    excludes: '.git/**,**/.git/**,Jenkinsfile',
                    fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Bike website pipeline completed successfully!'
        }
        failure {
            echo 'Bike website pipeline failed. Check the console log.'
        }
        always {
            echo 'Pipeline execution finished.'
        }
    }
}
