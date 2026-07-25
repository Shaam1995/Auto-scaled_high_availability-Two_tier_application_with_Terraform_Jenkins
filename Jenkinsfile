pipeline {
    agent any

    environment {
        WEB_SERVER_IP = "3.17.187.170"   // replace with your EC2 public IP
        WEB_SERVER_USER = "ubuntu"
        DEPLOY_PATH = "/usr/share/nginx/html"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: ' https://github.com/Shaam1995/Auto-scaled_high_availability-Two_tier_application_with_Terraform_Jenkins.git'
            }
        }

        stage('Stamp Build Number') {
            steps {
                sh "sed -i 's/placeholder/${env.BUILD_NUMBER}/' index.html"
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['dev']) {
                    sh """
                        scp -o StrictHostKeyChecking=no index.html ${WEB_SERVER_USER}@${WEB_SERVER_IP}:${DEPLOY_PATH}/index.html
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployed. Visit: http://${env.WEB_SERVER_IP}"
        }
        failure {
            echo "Deployment failed — check logs."
        }
    }
}

