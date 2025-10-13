pipeline {
    agent any   // Run on any available Jenkins agent

    environment {
        APP_ENV = 'dev'
        APP_NAME = 'my-sample-app'
    }

    options {
        timestamps()             // Show timestamps in console output
        skipDefaultCheckout()    // We'll manually checkout the repo
        buildDiscarder(logRotator(numToKeepStr: '10')) // Keep only 10 builds
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm   // Pulls code from the same repo where Jenkinsfile lives
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh '''
                    echo "Running build commands..."
                    mkdir -p build
                    echo "Build complete for ${APP_NAME}" > build/info.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                    echo "Starting tests..."
                    sleep 2
                    echo "All tests passed ✅"
                '''
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging application...'
                sh '''
                    mkdir -p output
                    tar -czf output/${APP_NAME}.tar.gz build/
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Archiving build output...'
                archiveArtifacts artifacts: 'output/*.tar.gz', fingerprint: true
            }
        }

        stage('Deploy (Optional)') {
            steps {
                echo 'Deploying application to environment...'
                sh 'echo "Deploying ${APP_NAME} to ${APP_ENV} environment"'
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Please check logs.'
        }
    }
}
