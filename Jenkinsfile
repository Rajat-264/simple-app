pipeline {
    agent any

    environment {
        IMAGE_NAME = 'rajathande/cicd-node-app'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/Rajat-264/simple-app.git'
            }
        }

        // Rest of your stages remain the same...
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(IMAGE_NAME)
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dckr_pat_ZUEmVaT43W1OKZAIXXWUU4-ko0k', url: '']) {
                    script {
                        docker.image(IMAGE_NAME).push('latest')
                    }
                }
            }
        }

        stage('Run Container (Optional)') {
            steps {
                sh """
                    docker stop cicd-node-app || true
                    docker rm cicd-node-app || true
                    docker run -d --name cicd-node-app -p 3000:3000 ${IMAGE_NAME}:latest
                """
            }
        }
    }
}