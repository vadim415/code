
pipeline {
    agent any
    //tools {
    //   terraform 'terraform'
    //}
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        TF_IN_AUTOMATION      = '1'
    }
    
    stages {
        stage('Git checkout') {
           steps{
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:vadim415/devops.git'
            }
        }
        //stages {
        stage("Run AWS thing part 1") {
            steps {
                withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding', 
                        credentialsId: 'aws_id', 
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'      
                ]]) {
                    sh "aws sts get-caller-identity"
            }
        }
        stage('terraform format check') {
            steps{
                sh 'terraform fmt'
            }
        }
        stage('terraform Init') {
            steps{
                sh 'terraform init'
                sh 'aws ls'
            }
        }
        stage('terraform apply') {
            steps{
                sh 'terraform apply --auto-approve'
            }
        }
    }

    
}
