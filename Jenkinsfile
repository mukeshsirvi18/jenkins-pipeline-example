pipeline {
    agent any

    tools {
        nodejs 'node-js' // Replace with your configured NodeJS version name
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/mukeshsirvi18/jenkins-pipeline-example' // Replace with your repo URL
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def app = docker.build("YOUR_DOCKERHUB_USERNAME/YOUR_IMAGE_NAME:${env.BUILD_ID}") // Update with your DockerHub username and image name
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') { // Replace with your DockerHub credentials ID
                        app.push()
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
            deleteDir() // Clean up workspace
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
