pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials' // Replace with your credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/DevOps-AWS-123/my-node-app.git', branch: 'master'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Docker Login') {
            steps {
                script {
                    // Login to Docker
                    docker.withRegistry('https://hub.docker.com/repositories/rajnages/', DOCKER_CREDENTIALS_ID) {
                        // You can add additional Docker commands here if needed
                    }
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t rajnages/my-node-app:latest .'
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to the Docker registry
                    sh 'docker push rajnages/my-node-app:latest'
                }
            }
        }
        
        stage('Deploy to Docker') {
            steps {
                sh 'docker run -d -p 3000:3000 rajnages/my-node-app:latest'
            }
        }
    }

post {
    success {
        // Send notification on successful build
        echo 'Build and Deployment Successful!'
        
        emailext(
            subject: "Jenkins Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "The build for ${env.JOB_NAME} was successful.\n\nCheck it out at: ${env.BUILD_URL}",
            recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'RequesterRecipientProvider']],
            to: 'rajavarapunages@gmail.com' // Replace with actual recipient email
        )
    }
    failure {
        // Send notification on build failure
        echo 'Build or Deployment Failed!'
        
        emailext(
            subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "The build for ${env.JOB_NAME} has failed.\n\nCheck the logs at: ${env.BUILD_URL}",
            recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'RequesterRecipientProvider']],
            to: 'rajavarapunages@gmail.com' // Replace with actual recipient email
        )
    }
}

}
