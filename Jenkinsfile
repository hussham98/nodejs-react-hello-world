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
                    def AWS_ACCESS_KEY_ID = credentials('deltablue-aws')
                    def AWS_SECRET_ACCESS_KEY = credentials('AKIA5TLHNTDSDLMZYTER')

                    withAWS(credentials: [
                        awsAccessKeyId(credentialsId: 'deltablue-aws', variable: 'AWS_ACCESS_KEY_ID'),
                        awsSecretKey(credentialsId: 'AKIA5TLHNTDSDLMZYTER', variable: 'AWS_SECRET_ACCESS_KEY')
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
pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-jenkins-demo')
        
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
                    export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
                    export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
                    
                    # Connect to your EC2 instance using SSH
                    ssh -i /home/robb/Downloads/deltablue.pem ec2-user@54.80.18.212

                    # Navigate to your app directory
                    cd /.

                    # Pull the latest code from GitHub
                    git pull origin main

                    # Install Node.js dependencies
                    npm install
                    npm run start
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
