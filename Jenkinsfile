pipeline {
    agent any

    
    stages {

        stage('Setup') {
            steps {
                sh "pip3 install -r lambda-app/tests/requirements.txt"
                sh "unzip aws-sam-cli-linux-x86_64.zip -d sam-installation"
                sh "sudo ./sam-installation/install"
                sh "sam --version"
            }
        }
        stage('Test') {
            steps {
                sh 'export PATH=$HOME/.local/bin:$PATH && pytest'
            }
        }

        stage('Build') {
            steps {
                sh "sam build -t lambda-app/template.yaml -y"

            }
        }
        stage('Deploy') {

            environment {
                AWS_ACCESS_KEY_ID = credentials('aws-access-key')
                AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
            }
            
            steps {
                sh "sam deploy -t lambda-app/template.yaml --no-confirm-changeset --no-fail-on-empty-changeset"

            }
        }
      
    }
}