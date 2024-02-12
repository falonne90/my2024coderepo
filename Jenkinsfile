pipeline {
  agent any

  parameters {
    string(name: 'BRANCH', defaultValue: 'master')
  }
    string(name: 'pro-stack', defaultValue: 'my-stack')
    string(name: 'REGION', defaultValue: 'us-east-1')
    string(name: 'ACTION', choices: ['create', 'update', 'delete'])
  }

   environment {
    credentialsId('aws-credentials-id', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')
    NETWORK_STACK_NAME= 'pro-stack'
    WEB_APP_STACK_NAME= 'app-stack'
    DATABASE_STACK_NAME= 'db-stack'
    SSM_STACK_NAME= 'ssm-stack'
    NETWORK_TEMPLATE_FILE= 'mynetwork.yaml'
    WEB_APP_TEMPLATE_FILE= 'mywebapp-elbcode.yaml'
    DATABASE_TEMPLATE_FILE= 'mydatabase.yaml' 
    SSM_TEMPLATE_FILE= 'myssm.yaml'         
  }

  stages {
    stage('Validate template') {
      steps {
          // Install cfn-lint
          sh 'pip install cfn-lint'
      }
    }

    stage('Checkout code') {
      steps {
        checkout([
          branch: params.BRANCH,
          scm: [$class: 'Git', url: 'https://github.com/falonne90/my2024coderepo']
        ])
      }
    }
  }

    stages {
    stage('Deploy CloudFormation') {
      steps {
        script {
          withCredentials([[$lass: 'AmazonWbServicesCredentialsBinding', accesskeyvariables: 'AWS_ACCESS_KEY_ID', credentialsId: 'newjenkins-user', secretkeyvariable: 'AWS_SECRET_ACCESS_KEY']])
            sh "aws cloudformation deploy --stack-name NETWORK_STACK_NAME --template-file NETWORK_TEMPLATE_FILE --region \"${AWS_REGION}\""
            sh "aws cloudformation deploy --stack-name WEB_APP_STACK_NAME --template-file mWEB_APP_TEMPLATE_FILE --region \"${AWS_REGION}\""
            sh "aws cloudformation deploy --stack-name SSM_STACK_NAME= 'ssm-stack' --template-file SSM_TEMPLATE_FILE --capabilities CAPABILITY_IAM --IAM --region \"${AWS_REGION}\""
            sh "aws cloudformation deploy --stack-name DATABASE_STACK_NAME --template-file DATABASE_TEMPLATE_FILE --region \"${AWS_REGION}\""
          }
        }
      }
    }

   Post {
     success {
       echo 'Deployment succeeded, congrats Falonne!'
     }
     failure {
       echo 'Falonne check your template, Deployment failed'
     }
   }


// #Remember to add pre-build, build and artifacts