pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/shadaab29/fashion_website.git',
                        credentialsId: 'demo1' // Make sure this exists
                    ]]
                ])
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t static-website .'
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                script {
                    def containerExists = bat(script: 'docker ps -q --filter "name=static-website"', returnStdout: true).trim()
                    if (containerExists) {
                        bat 'docker stop static-website'
                        bat 'docker rm static-website'
                    }
                }
            }
        }

        stage('Run New Container') {
            steps {
                bat 'docker run -d -p 8081:80 --name static-website static-website'
            }
        }
    }
}
