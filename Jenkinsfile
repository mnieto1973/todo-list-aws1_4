pipeline {
    agent any

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
                sh 'sam deploy --config-env --parameter-overrides staging Stage=staging'
            }
        }
        stage('Get Stack URL') {
            steps {
                script {
                    def url = sh(script: "aws cloudformation describe-stacks --stack-name ${STACK_NAME} --query \"Stacks[0].Outputs[?OutputKey=='<YourOutputKeyName>'].OutputValue\" --output text", returnStdout: true).trim()
                    env.STACK_URL = url
                    echo "Stack URL: ${STACK_URL}"
                }
            }
        }
    }
}