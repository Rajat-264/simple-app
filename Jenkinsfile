pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
    - name: node
      image: node:18
      command:
        - cat
      tty: true
"""
        }
    }
    stages {
        stage('Install') {
            steps {
                container('node') {
                    sh 'npm install'
                }
            }
        }
        stage('Test') {
            steps {
                container('node') {
                    sh 'npm test'
                }
            }
        }
        stage('Build') {
            steps {
                container('node') {
                    sh 'npm run build'
                }
            }
        }
    }
}
