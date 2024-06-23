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
    }
}