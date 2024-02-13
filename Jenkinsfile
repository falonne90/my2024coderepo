pipeline {
    agent any

    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Git branch to deploy')
        string(name: 'REGION', defaultValue: 'us-east-1', description: 'AWS Region')
    }
  
    environment {
        AWS_REGION = 'params.REGION'
        NETWORK_STACK_NAME = 'pro-stack'
        WEB_APP_STACK_NAME = 'app-stack'
        DATABASE_STACK_NAME = 'db-stack'
        SSM_STACK_NAME = 'ssm-stack'
        NETWORK_TEMPLATE_FILE = 'mynetwork.yaml'
        WEB_APP_TEMPLATE_FILE = 'mywebapp-elbcode.yaml'
        DATABASE_TEMPLATE_FILE = 'mydatabase.yaml'
        SSM_TEMPLATE_FILE = 'myssm.yaml'

        // PYTHON_HOME = tool 'Python3' // Assuming you have a Python tool configured
        // PATH = "${PYTHON_HOME}/bin:${env.PATH}"
        // AWS_CLI_HOME = tool 'AWS_CLI' // Assuming you have a tool configured for AWS CLI
        // PATH = "${AWS_CLI_HOME}:${env.PATH}"
    }

    stages {
        // stage('Install AWS CLI') {
        //     steps {
        //         script {
        //             echo 'Installing AWS CLI...'
        //             sh 'pip install awscli --upgrade --user'
        //         }
        //     }
        // }

        stage('Validate template') {
            steps {
                script {
                    echo 'Installing cfn-lint...'
                    sh 'pip install cfn-lint'
                }
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

    //     stage('Validate template') {
    //         steps {
    //             script {
    //                 // Install cfn-lint
    //                 sh 'pip install cfn-lint'
    //             }
    //         }
    //     }

        // stage('Checkout code') {
        //     steps {
        //         checkout([
        //             branch: params.BRANCH,
        //             scm: [$class: 'Git', url: 'https://github.com/falonne90/my2024coderepo']
        //         ])
        //     }
        // }

        stage('Deploy CloudFormation') {
            steps {
                script {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'newjenkins-user', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        sh "aws cloudformation deploy --stack-name ${NETWORK_STACK_NAME} --template-file ${NETWORK_TEMPLATE_FILE} --region ${AWS_REGION}"
                        sh "aws cloudformation deploy --stack-name ${WEB_APP_STACK_NAME} --template-file ${WEB_APP_TEMPLATE_FILE} --region ${AWS_REGION}"
                        sh "aws cloudformation deploy --stack-name ${SSM_STACK_NAME} --template-file ${SSM_TEMPLATE_FILE} --capabilities CAPABILITY_IAM --region ${AWS_REGION}"
                        sh "aws cloudformation deploy --stack-name ${DATABASE_STACK_NAME} --template-file ${DATABASE_TEMPLATE_FILE} --region ${AWS_REGION}"
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



// #Remember to add pre-build, build and artifacts 


