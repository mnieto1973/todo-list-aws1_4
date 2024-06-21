pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                git branch: 'develop', url: 'https://github.com/mnieto1973/todo-list-aws1_4.git'
            }
        }
    }
}