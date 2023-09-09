pipeline {
    agent any
    parameters {
          parameters {
        string(name: 'AMI_ID', description: 'Amazon Machine Image (AMI) ID')
        string(name: 'INSTANCE_TYPE', description: 'EC2 Instance Type')
    }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Clone Terraform Scripts') {
            steps {
                // Run any commands to clone your Terraform scripts
                sh 'git clone https://github.com/DivyaReddy0630/task2.git'
            }
        }

                       script {
                    def amiId = params.AMI_ID
                    def instanceType = params.INSTANCE_TYPE
                    def awsRegion = params.AWS_REGION

                    // Assume the AWS IAM Role using AWS CLI
                    sh 'aws sts assume-role --role-arn arn:aws:iam::YOUR_ACCOUNT_ID:role/YOUR_ROLE_NAME --role-session-name jenkins-session > assumed-credentials.json'

                    // Set AWS environment variables using the assumed credentials
                    withCredentials([file(credentialsBinding: 'assumed-credentials.json', variable: 'AWS_CREDENTIALS_JSON')]) {
                        sh """
                        export AWS_ACCESS_KEY_ID=$(jq -r '.Credentials.AccessKeyId' <<< $AWS_CREDENTIALS_JSON)
                        export AWS_SECRET_ACCESS_KEY=$(jq -r '.Credentials.SecretAccessKey' <<< $AWS_CREDENTIALS_JSON)
                        export AWS_SESSION_TOKEN=$(jq -r '.Credentials.SessionToken' <<< $AWS_CREDENTIALS_JSON)
                        export AWS_DEFAULT_REGION=${awsRegion}
                        """
                    }
        stage('Terraform Init & Apply') {
            steps {
                // Change to the directory containing your Terraform scripts
                dir('terraform-repo') {
                    sh 'terraform init'
                    sh 'terraform apply -auto-approve'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
            // You can add post-pipeline actions here.
        }

        failure {
            echo 'Pipeline failed!'
            // You can add actions to take in case of pipeline failure.
        }
    }
}
