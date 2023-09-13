pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build and Deploy') {
            steps {
                script {
                    withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'deltablue-aws', // The ID of your AWS credentials in Jenkins
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',     // Environment variable to store the AWS access key
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY' // Environment variable to store the AWS secret key
                ]]) {
                    ]) {
                        sh """
                        # Connect to your EC2 instance using SSH
                        ssh -i "deltablue.pem" ubuntu@ec2-174-129-66-64.compute-1.amazonaws.com

                        # Navigate to your app directory
                        cd /

                        # Pull the latest code from GitHub
                        git pull origin main

                        # Install Node.js dependencies
                        npm install
                        npm run start
                        npm run build
                        npm run bundle

                        # Restart your Node.js app
                        pm2 nodejs-react-hello-world
                        """
                    }
                }
            }
        }
    }
}
