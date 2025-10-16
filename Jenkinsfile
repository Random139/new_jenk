pipeline {
  agent any
  environment {
    AWS_REGION = 'ap-southeast-2' 
    BUCKET_NAME = "my-sync-bucket139"
    ROLE_ARN = 'arn:aws:iam::873046390774:role/S3FullAccess'
    ROLE_SESSION_NAME = 'JenkinsSession'
  }
  stages {
    stage('Assume Role') {
      steps {
        script {
          def assumeRoleOutput = sh(
            script: """
              aws sts assume-role --role-arn ${ROLE_ARN} --role-session-name ${ROLE_SESSION_NAME} --region ${AWS_REGION}
            """,
            returnStdout: true
          ).trim()

          def credentials = readJSON text: assumeRoleOutput
          env.AWS_ACCESS_KEY_ID = credentials.Credentials.AccessKeyId
          env.AWS_SECRET_ACCESS_KEY = credentials.Credentials.SecretAccessKey
          env.AWS_SESSION_TOKEN = credentials.Credentials.SessionToken
        }
      }
    }

    stage('Prepare Bucket') {
      steps {
        script {
          def exists = sh(
            script: "aws s3api head-bucket --bucket ${BUCKET_NAME} --region ${AWS_REGION} 2>/dev/null",
            returnStatus: true
          )
          if (exists != 0) {
            sh """
              aws s3api create-bucket \\
                --bucket ${BUCKET_NAME} \\
                --region ${AWS_REGION} \\
                --create-bucket-configuration LocationConstraint=${AWS_REGION}
            """
            echo "Bucket ${BUCKET_NAME} created"
          } else {
            echo "Bucket ${BUCKET_NAME} already exists"
          }
        }
      }
    }

    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/Random139/new_jenk.git'
      }
    }

    stage('Sync to S3') {
      steps {
        script {
          sh """
            aws s3 sync . s3://${BUCKET_NAME} --region ${AWS_REGION} --delete
          """
        }
      }
    }
  }

  post {
    success {
      echo "SUCCESS: Repository synced to s3://${BUCKET_NAME}"
    }
    failure {
      echo "FAILURE: Could not sync to S3"
    }
    always {
      echo "Pipeline finished (build #${env.BUILD_NUMBER})"
    }
  }
}
