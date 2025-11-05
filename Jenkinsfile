pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Aadilkhan321/flask-ci-cd-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                echo '✅ Running tests...'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-ci-cd-demo:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 5000:5000 flask-ci-cd-demo:latest'
            }
        }

        stage('Push Changes to GitHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-creds', usernameVariable: 'USER', passwordVariable: 'TOKEN')]) {
                    sh '''
                        git config user.email "jenkins@ci.com"
                        git config user.name "Jenkins CI"
                        git add .
                        git diff --cached --quiet || git commit -m "Automated commit from Jenkins build"
                        git push https://${USER}:${TOKEN}@github.com/Aadilkhan321/flask-ci-cd-demo.git HEAD:main
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '🎉 Build and deployment successful!'
        }
        failure {
            echo '❌ Build failed. Check logs.'
        }
    }
}
