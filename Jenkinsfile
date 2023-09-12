pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('your-aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('your-aws-secret-access-key')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build and Deploy') {
            steps {
                script {
                    sh """
                    # Set AWS credentials as environment variables
                    export AWS_ACCESS_KEY_ID=${AKIA5TLHNTDSGZ5AEFH6}
                    export AWS_SECRET_ACCESS_KEY=${t0iz37Di37Hwh2qMl1vMUyK/AFYmCRJ924mOh7Fb}
                    
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
                    """
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
                    // Retrieve AWS credentials from Jenkins secret text
                    def awsAccessKeyId = credentials('0e52fb54-0392-440b-9043-a127945b615d')
                    def awsSecretAccessKey = credentials('AKIA5TLHNTDSGZ5AEFH6')

                    // Set AWS credentials for this stage
                    withAWS(region: 'us-east-1', credentials: [
                        awsAccessKeyId(credentialsId: '0e52fb54-0392-440b-9043-a127945b615d', variable: 'AWS_ACCESS_KEY_ID'),
                        awsSecretKey(credentialsId: '0e52fb54-0392-440b-9043-a127945b615d', variable: 'AWS_SECRET_ACCESS_KEY')
                    ]) {
                        // Connect to your EC2 instance using SSH
                        sh """
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
                        EOF
                        """
                    }
                }
            }
        }
    }
}

