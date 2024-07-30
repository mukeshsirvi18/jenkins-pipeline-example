pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone your repository
                git 'https://github.com/mukeshsirvi18/jenkins-pipeline-example' // Replace with your repo URL
            }
        }
        stage('Install Node.js') {
            steps {
                // Install Node.js (ensure it's available on the Jenkins agent)
                sh '''
                curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
                sudo apt-get install -y nodejs
                '''
            }
        }
        stage('Build') {
            steps {
                // Install npm dependencies and build your application
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                // Run your tests
                sh 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    def app = docker.build("YOUR_DOCKERHUB_USERNAME/YOUR_IMAGE_NAME:${env.BUILD_ID}") // Update with your DockerHub username and image name
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
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
