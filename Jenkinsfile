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
                sh 'echo "Hello">text.txt'
                sh 'cat text.txt'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "publish"' 
            }
        }
    }
}
