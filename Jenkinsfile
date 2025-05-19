pipeline {
    agent any

    environment {
        IMAGE_NAME = 'rajathande/cicd-node-app'
    }

    stages {
        stage('Clone') {
            steps {
                checkout([$class: 'GitSCM', 
                         branches: [[name: 'main']],
                         userRemoteConfigs: [[url: 'https://github.com/Rajat-264/simple-app.git']]])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(IMAGE_NAME)
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dckr_pat_ZUEmVaT43W1OKZAIXXWUU4-ko0k') {
                        docker.image(IMAGE_NAME).push('latest')
                    }
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh "docker stop cicd-node-app || true"
                    sh "docker rm cicd-node-app || true"
                    sh "docker run -d --name cicd-node-app -p 3000:3000 ${IMAGE_NAME}:latest"
                }
            }
        }
    }
}
