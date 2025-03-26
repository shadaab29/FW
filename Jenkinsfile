pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/shadaab29/fashion_website.git' // Replace with your repository URL
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t static-website .'
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sh '''
                docker stop static-website || true
                docker rm static-website || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 80:80 --name static-website static-website'
            }
        }
    }
}
