// MY template below , i need to troubleshoot this 
pipeline {
    agent any

    // parameters {
    //     string(name: 'BRANCH', defaultValue: 'master', description: 'Git branch to deploy')
    //     string(name: 'REGION', defaultValue: 'us-east-1', description: 'AWS Region')
    // }
  
    environment {
        REGION = 'us-east-1'
        NETWORK_STACK_NAME = 'pro-stack'
        WEB_APP_STACK_NAME = 'app-stack'
        DATABASE_STACK_NAME = 'db-stack'
        SSM_STACK_NAME = 'ssm-stack'
        NETWORK_TEMPLATE_FILE = 'mynetwork.yaml'
        WEB_APP_TEMPLATE_FILE = 'mywebapp-elbcode.yaml'
        DATABASE_TEMPLATE_FILE = 'mydatabase.yaml'
        SSM_TEMPLATE_FILE = 'myssm.yaml'
    }

    stages {
        // stage('Validate template') {
        //     steps {
        //         script {
        //             echo 'Installing cfn-lint...'
        //             sh 'pip install cfn-lint'
        //         }
        //     }
        // }
        // stage('Checkout code') {
        //     steps {
        //         checkout([$class: 'Git', branches: [[name: params.BRANCH]], userRemoteConfigs: [[url: 'https://github.com/falonne90/my2024coderepo']]])
        //     }
        // }

        stage('Deploy CloudFormation') {
            steps {
                script {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'newjenkins-user', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        sh "aws cloudformation deploy --stack-name $NETWORK_STACK_NAME --template-file $NETWORK_TEMPLATE_FILE --region $REGION"
                        sh "aws cloudformation deploy --stack-name $WEB_APP_STACK_NAME --template-file $WEB_APP_TEMPLATE_FILE --region $REGION"
                        sh "aws cloudformation deploy --stack-name $SSM_STACK_NAME --template-file $SSM_TEMPLATE_FILE --capabilities CAPABILITY_IAM --region $REGION"
                        sh "aws cloudformation deploy --stack-name $DATABASE_STACK_NAME --template-file $DATABASE_TEMPLATE_FILE --region $REGION"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded, congrats Falonne!'
        }
        failure {
            echo 'Falonne, check your template. Deployment failed.'
        }
    }
}




// pipeline {
//   agent any
//   environment {
//     AWS_REGION = 'us-east-1'
//   }
//   stages {
//     stage('Deploy to CloudFormation') {
//       steps {
//         script {
//           withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'newjenkins-user', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
//             sh "aws cloudformation deploy --stack-name pro-stack --template-file mynetwork.yaml --region \"${AWS_REGION}\""
//             sh "aws cloudformation deploy --stack-name app-stack --template-file mywebapp-elbcode.yaml --region \"${AWS_REGION}\""
//             sh "aws cloudformation deploy --stack-name db-stack --template-file mydatabase.yaml --region \"${AWS_REGION}\""
//             sh "aws cloudformation deploy --stack-name ssm-stack --template-file myssm.yaml --capabilities CAPABILITY_IAM --region \"${AWS_REGION}\""
//         }
//       }
//     }
//     }
//   }

//   post {
//     success {
//       echo 'Falonne, your Deployment succeeded!'
//     }
//     failure {
//       echo 'Falonne, your Deployment failed!'
//     }
//   }
// }

