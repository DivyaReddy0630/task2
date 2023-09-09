pipeline {
    agent any
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
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
                sh 'git clone https://github.com/yourusername/terraform-repo.git'
            }
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
