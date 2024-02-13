pipeline {
  agent any
  environment {
    AWS_REGION = 'us-east-1'
  }
  stages {
    stage('Deploy to CloudFormation') {
      steps {
        script {
          withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'newjenkins-user', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
            // sh "aws cloudformation deploy --stack-name pro-stack --template-file mynetwork.yaml --region \"${AWS_REGION}\""
            // sh "aws cloudformation deploy --stack-name app-stack --template-file mywebapp-elbcode.yaml --region \"${AWS_REGION}\""
            sh "aws cloudformation deploy --stack-name db-stack --template-file mydatabase.yaml --region \"${AWS_REGION}\""
            sh "aws cloudformation deploy --stack-name ssm-stack --template-file myssm.yaml --capabilities CAPABILITY_IAM --region \"${AWS_REGION}\""
        }
      }
    }
    }
  }

  post {
    success {
      echo 'Danielle, your Deployment succeeded!'
    }
    failure {
      echo 'Mme F, your Deployment failed!'
    }
  }
}

