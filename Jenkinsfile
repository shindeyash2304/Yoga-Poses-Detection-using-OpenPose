pipeline {
    agent any
    environment {
        // Define environment variables for reusable configuration
        IMAGE_NAME = 'flask-yoga-app'
        CONTAINER_PORT = '5000'
        HOST_PORT = '5003'
        RECIPIENT_EMAIL = 'siddharthvermaofficial3105@gmail.com' // Replace with the actual recipient email
    }
    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Cloning the repository...'
                git branch: 'main', url: 'https://github.com/Sid31052002/flask-yoga-app.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying the application...'
                // Stop any running container with the same name (if necessary)
                sh "docker ps -q --filter ancestor=${IMAGE_NAME} | xargs -r docker stop"
                // Run the Docker container
                sh "docker run -d -p ${HOST_PORT}:${CONTAINER_PORT} ${IMAGE_NAME}"
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution completed!'
        }
        success {
            echo 'Deployment succeeded.'
            emailext(
                subject: "SUCCESS: Jenkins Pipeline - ${env.JOB_NAME} (#${env.BUILD_NUMBER})",
                body: """<p>The pipeline for <b>${env.JOB_NAME}</b> completed successfully!</p>
                         <p>Build Number: ${env.BUILD_NUMBER}</p>
                         <p><a href="${env.BUILD_URL}">Click here</a> to view the build details.</p>""",
                to: "${RECIPIENT_EMAIL}",
                mimeType: 'text/html'
            )
        }
        failure {
            echo 'Deployment failed.'
            emailext(
                subject: "FAILURE: Jenkins Pipeline - ${env.JOB_NAME} (#${env.BUILD_NUMBER})",
                body: """<p>The pipeline for <b>${env.JOB_NAME}</b> failed.</p>
                         <p>Build Number: ${env.BUILD_NUMBER}</p>
                         <p><a href="${env.BUILD_URL}">Click here</a> to view the build details and logs.</p>""",
                to: "${RECIPIENT_EMAIL}",
                mimeType: 'text/html'
            )}
            }
}