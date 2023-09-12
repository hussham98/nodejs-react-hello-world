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
                    def AWS_ACCESS_KEY_ID = credentials('your-aws-access-key-id')
                    def AWS_SECRET_ACCESS_KEY = credentials('your-aws-secret-access-key')

                    withAWS(credentials: [
                        awsAccessKeyId(credentialsId: 'your-aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                        awsSecretKey(credentialsId: 'your-aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                    ]) {
                        sh """
                        # Connect to your EC2 instance using SSH
                        ssh -i /home/robb/Downloads/deltablue.pem ec2-user@54.80.18.212

                        # Navigate to your app directory
                        cd /

                        # Pull the latest code from GitHub
                        git pull origin main

                         # Install Node.js dependencies
                        npm install
                        npm run build
                        npm run bundle

                        # Restart your Node.js app
                        pm2 restart nodejs-react-hello-world
                        """
                    }
                }
            }
        }
    }
}
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
                    // Define the secret file containing AWS credentials
                    def awsCredentialsFile = credentials('your-aws-credentials-file-id')

                    // Read the contents of the secret file
                    def awsCredentials = readFile awsCredentialsFile

                    // Split the contents into Access Key ID and Secret Access Key
                    def credentialsList = awsCredentials.split('\n')

                    if (credentialsList.size() != 2) {
                        error('Invalid AWS credentials format in the secret file.')
                    }

                    def AWS_ACCESS_KEY_ID = credentialsList[0].trim()
                    def AWS_SECRET_ACCESS_KEY = credentialsList[1].trim()

                    withAWS(credentials: [
                        awsAccessKeyId(credentialsId: 'your-aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                        awsSecretKey(credentialsId: 'your-aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                    ]) {
                        sh """
                        # Connect to your EC2 instance using SSH
                        ssh -i /home/robb/Downloads/deltablue.pem ec2-user@54.80.18.212
                        # Navigate to your app directory
                        cd /path/to/your/app

                        # Pull the latest code from GitHub
                        git pull origin main

                        # Install Node.js dependencies
                        npm install
                        npm run build
                        npm run bundle

                        # Restart your Node.js app
                        pm2 restart nodejs-react-hello-world

                        """
                    }
                }
            }
        }
    }
}
