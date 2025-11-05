pipeline {
    agent {
        docker {
            image 'node:20'
            args '-u root:root'
        }
    }

    environment {
        PROJECT_NAME = "Node.js EC2 Jenkins Pipeline"
        AWS_REGION   = "us-east-1"
        S3_BUCKET    = "jenkins-nodejs-bucket"
        BUILD_OUTPUT = "."
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out code..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing npm dependencies..."
                sh 'npm install'
            }
        }

        stage('Upload to S3') {
            steps {
                echo "Uploading app to S3..."
                sh '''
                    apt-get update && apt-get install -y awscli

                    if ! aws s3 ls "s3://${S3_BUCKET}" >/dev/null 2>&1; then
                        echo "Creating bucket: ${S3_BUCKET}"
                        aws s3 mb s3://${S3_BUCKET} --region ${AWS_REGION}
                    else
                        echo "Bucket ${S3_BUCKET} already exists."
                    fi

                    echo "Uploading files..."
                    aws s3 cp ${BUILD_OUTPUT} s3://${S3_BUCKET}/${JOB_NAME}/${BUILD_NUMBER}/ --recursive --region ${AWS_REGION}
                '''
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
        success {
            echo "${PROJECT_NAME} completed successfully and uploaded to S3!"
        }
        failure {
            echo "${PROJECT_NAME} failed — check logs!"
        }
    }
}
