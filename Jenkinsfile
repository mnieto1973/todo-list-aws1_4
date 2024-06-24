pipeline {
    agent any
environment {
        STACK_NAME = 'todo-list-aws-1-4-staging'
    }
    stages {
        stage('Get Code') {
            steps {
                git branch: 'develop', url: 'https://github.com/mnieto1973/todo-list-aws1_4.git'
            }
        }
        stage('Static Test') {
            steps {
                sh 'flake8 src/ > flake8-report.txt'
                sh 'bandit -r src/ > bandit-report.txt'
                archiveArtifacts artifacts: '**/*-report.txt', allowEmptyArchive: true
            }
        }
        stage('Deploy') {
            steps {
                sh 'sam build'
                sh 'sam deploy --config-env staging --parameter-overrides Stage=staging --capabilities CAPABILITY_NAMED_IAM'
            }
        }
        stage('Get Stack URL') {
            steps {
                script {
                    def url = sh(script: "aws cloudformation describe-stacks --stack-name ${STACK_NAME} --query \"Stacks[0].Outputs[?OutputKey=='BaseUrlApi'].OutputValue\" --output text", returnStdout: true).trim()
                    env.STACK_URL = url
                    echo "Stack URL: ${STACK_URL}"
                }
            }
        }
         stage('Test') {
           steps {
                script {
                    export BASE_URL=${STACK_URL}
                    pytest --junitxml=result.rest.xml test\integration\todoApiTest.py
                }
                junit 'result*.xml'
            }
        }
    }
}