pipeline {
    agent any

    parameters {
        string(name: 'ami_id', description: 'Amazon Machine Image (AMI) ID')
        string(name: 'instancetype', description: 'EC2 Instance Type')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Terraform Init & Apply') {
            steps {
                script {
                    def ami_id = params.ami_id
                    def instancetype = params.instancetype

                    // Change to the directory containing your main.tf
                    dir('path/to/terraform/config') {
                        // Initialize Terraform
                        sh 'terraform init'

                        // Apply Terraform with variables from parameters
                        sh "terraform apply -auto-approve -var 'aws_region=${aws_region}' -var 'ami_id=${ami_id}' -var 'instance_type=${instance_type}'"
                    }
                }
            }
        }
        
        // Add more stages for additional tasks if needed.
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
