pipeline { actions
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
                sh 'docker build -t static-website .'
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                script {
                    def containerExists = sh(script: 'docker ps -q --filter "name=static-website"', returnStdout: true).trim()
                    if (containerExists) {
                        sh 'docker stop static-website'
                        sh 'docker rm static-website'
                    }
                }
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 8081:80 --name static-website static-website'
            }
        }
    }
}
