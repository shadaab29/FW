pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/shadaab29/fashion_website.git',
                        credentialsId: 'demo1' // Your Jenkins credentials ID
                    ]]
                ])
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
