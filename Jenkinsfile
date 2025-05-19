pipeline {
    agent any
    
    environment {
        IMAGE_NAME = 'rajathande/cicd-node-app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout([$class: 'GitSCM', 
                         branches: [[name: 'main']],
                         userRemoteConfigs: [[url: 'https://github.com/Rajat-264/simple-app.git']]])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Verify Docker is available
                    sh 'docker --version'
                    
                    // Build the image using shell commands
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'dckr_pat_ZUEmVaT43W1OKZAIXXWUU4-ko0k',
                        passwordVariable: 'DOCKER_PASSWORD',
                        usernameVariable: 'DOCKER_USERNAME'
                    )]) {
                        sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "docker push ${IMAGE_NAME}:latest"
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

    post {
        always {
            cleanWs()
        }
    }
}
