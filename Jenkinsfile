pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo "In Build"'
            }
        }
        stage('Test'){
            steps {
                sh 'echo "Hello"'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "publish"' 
            }
        }
    }
}
