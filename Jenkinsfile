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
                    if [ ! -f ./terraform ]; then
                        TF_VERSION=1.9.8
                        curl -sSLo terraform.zip https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip
                        jar xf terraform.zip
                        chmod +x terraform
                        rm -f terraform.zip
                    fi
                    ./terraform -version
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                withAWS(credentials: 'my-cba-aws-credential', region: 'eu-west-2') {
                    sh './terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                withAWS(credentials: 'my-cba-aws-credential', region: 'eu-west-2') {
                    sh './terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                withAWS(credentials: 'my-cba-aws-credential', region: 'eu-west-2') {
                    sh './terraform apply -auto-approve tfplan'
                }
            }
        }
    }
}