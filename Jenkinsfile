pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                
                git branch: 'main', url: 'https://github.com/joush1/nexgen-s3-terraform.git'
            }
        }

        stage('Install Terraform (if missing)') {
            steps {
                sh '''
                    if ! command -v terraform >/dev/null 2>&1; then
                        apt-get update && apt-get install -y wget unzip
                        wget -q https://releases.hashicorp.com/terraform/1.9.8/terraform_1.9.8_linux_amd64.zip
                        unzip -o terraform_1.9.8_linux_amd64.zip -d /usr/local/bin
                    fi
                    terraform -version
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                withAWS(credentials: 'my-cba-aws-credential', region: 'eu-west-2') {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                withAWS(credentials: 'my-cba-aws-credential', region: 'eu-west-2') {
                    sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                withAWS(credentials: 'my-cba-aws-credential', region: 'eu-west-2') {
                    sh 'terraform apply -auto-approve tfplan'
                }
            }
        }
    }
}