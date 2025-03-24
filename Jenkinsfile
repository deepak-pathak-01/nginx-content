pipeline {
    agent any
    environment {
        GIT_REPO = 'https://github.com/deepak-pathak-01/nginx-content.git'
        AWS_SERVER = 'ubuntu@13.233.61.14'
        AZURE_SERVER = 'azureuser@172.173.142.39'
        AWS_KEY = '/var/lib/jenkins/.ssh/aws-key1.pem'
        AZURE_KEY = '/var/lib/jenkins/.ssh/MyVM_key.pem'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }
        stage('Deploy to AWS') {
            steps {
                script {
                    sh """
                    chmod 600 ${AWS_KEY}
                    scp -o StrictHostKeyChecking=no -i ${AWS_KEY} index-aws.html ${AWS_SERVER}:/home/ubuntu/index.html
                    ssh -o StrictHostKeyChecking=no -i ${AWS_KEY} ${AWS_SERVER} 'sudo mv /home/ubuntu/index.html /var/www/html/index.html && sudo systemctl restart nginx'
                    """
                }
            }
        }
        stage('Deploy to Azure') {
            steps {
                script {
                    sh """
                    chmod 600 ${AZURE_KEY}
                    scp -o StrictHostKeyChecking=no -i ${AZURE_KEY} index-azure.html ${AZURE_SERVER}:/home/azureuser/index.html
                    ssh -o StrictHostKeyChecking=no -i ${AZURE_KEY} ${AZURE_SERVER} 'sudo mv /home/azureuser/index.html /var/www/html/index.html && sudo systemctl restart nginx'
                    """
                }
            }
        }
    }
    post {
        success {
            echo "Deployment to AWS and Azure completed successfully!"
        }
        failure {
            echo "Deployment failed! Check logs for errors."
        }
    }
}
