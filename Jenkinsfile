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
                sh 'flake8 src/ > flake8-report.out'
                 recordIssues tools: [flake8(name: 'Flake8', pattern: 'flake8-report.out')], qualityGates: [[threshold:8, type: 'TOTAL', unstable: true], [threshold: 10, type: 'TOTAL', unstable: false]]
                sh 'bandit -r src/ > bandit-report.out'
                recordIssues tools: [pyLint(name: 'Bandit', pattern: 'bandit-report.out')], qualityGates: [[threshold:2, type: 'TOTAL', unstable: true], [threshold: 4, type: 'TOTAL', unstable: false]]
            }
        }
    }
}