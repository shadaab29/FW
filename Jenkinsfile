pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/shadaab29/fashion_website.git',
                        credentialsId: 'demo1' // Make sure this exists in Jenkins credentials
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
                    echo 'Checking for existing "static-website" container...'
                    def containerExists = bat(
                        script: 'docker ps -a -q --filter "name=static-website"',
                        returnStdout: true
                    ).trim()

                    if (containerExists) {
                        echo 'Stopping and removing old container...'
                        bat 'docker stop static-website'
                        bat 'docker rm static-website'
                    } else {
                        echo 'No existing container to stop/remove.'
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

    post {
        success {
            echo '✅ Pipeline executed successfully.'
        }
        failure {
            echo '❌ Pipeline failed. Please check logs above.'
        }
    }
}
