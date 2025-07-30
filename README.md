pipeline {
    agent any
    stages {
        stage ("checkout"){
            steps{
            git branch: 'main', url: 'https://github.com/diptendupesre/diptendupersonalProject.git'
        }
        }
        stage('check aws cli') {
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                sh 'aws --version'
                sh 'aws ec2 describe-instances'
                sh 'terraform version'
                }
            }
	}
        stage('Terraform init'){
        steps{
             withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
        sh 'terraform init'
		sh 'terraform plan'
		sh 'terraform apply —-auto-approve'
             }
        }
        }
        }
    }
