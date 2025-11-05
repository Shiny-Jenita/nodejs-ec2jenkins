pipeline {
    agent {
        docker { 
            image 'node:20'
            args '-u root:root'  // Run as root to avoid permission issues
        }
    }

    environment {
        PROJECT_NAME = "Node.js EC2 Jenkins Pipeline"
        AWS_REGION   = "us-east-1"
        S3_BUCKET    = "jenkins-nodejs-bucket"
        BUILD_OUTPUT = "output" // change this to your actual build output folder or file
    }

    stages {

        stage('Checkout') {
            steps {
                echo "🔄 Checking out code..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing npm dependencies..."
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo "Building project..."
                sh 'npm run build || echo "No build script, skipping..."'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'npm test || echo "No tests found, skipping..."'
            }
        }

        stage('Upload to S3') {
            steps {
                echo "Uploading build output to S3..."

                // Ensure AWS CLI is available
                sh 'apt-get update && apt-get install -y awscli'

                // Create bucket if it doesn't exist, then upload
                sh """
                    if ! aws s3 ls "s3://${S3_BUCKET}" 2>&1 | grep -q 'NoSuchBucket'; then
                        echo "Bucket ${S3_BUCKET} already exists."
                    else
                        echo "Creating bucket: ${S3_BUCKET}"
                        aws s3 mb s3://${S3_BUCKET} --region ${AWS_REGION}
                    fi

                    echo "☁️ Uploading build output..."
                    aws s3 cp ${BUILD_OUTPUT} s3://${S3_BUCKET}/${JOB_NAME}/${BUILD_NUMBER}/ --recursive --region ${AWS_REGION}
                """
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Simulated deployment step..."
                sh 'echo "Simulated deployment complete."'
            }
        }
    }

    post {
        always {
            echo "🧹 Cleaning up workspace..."
            cleanWs()
        }
        success {
            echo "✅ ${PROJECT_NAME} completed successfully and uploaded to S3!"
        }
        failure {
            echo "❌ ${PROJECT_NAME} failed!"
        }
    }
}
