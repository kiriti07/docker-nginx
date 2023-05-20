pipeline {
    agent any
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build('my-nginx-image')
                    dockerImage.inside {
                        sh 'apt-get update && apt-get install -y nginx'
                    }
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        def dockerImage = docker.image('my-nginx-image')
                        dockerImage.push('latest')
                    }
                }
            }
        }
        
        stage('Run NGINX') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image('my-nginx-image').run('-p 80:80')
                    }
                }
            }
        }
    }
}
