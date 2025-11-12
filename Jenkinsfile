pipeline {
  agent any

  environment {
    REPO_URL  = 'https://github.com/MSajid44/Expense-Tracker.git'
    APP_NAME  = 'expense-tracker'
    IMAGE_TAG = "${APP_NAME}:${BUILD_NUMBER}"
    IMAGE_TAR = "${WORKSPACE}/${APP_NAME}-${BUILD_NUMBER}.tar"
  }

  stages {
    stage('Checkout') {
      steps {
        git url: "${REPO_URL}", branch: 'main'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          echo "Building Docker image..."
          docker build -t ${IMAGE_TAG} .
        '''
      }
    }

    stage('Save Image to Workspace') {
      steps {
        sh '''
          echo "Saving Docker image to workspace..."
          docker save -o ${IMAGE_TAR} ${IMAGE_TAG}
          ls -lh ${WORKSPACE}
        '''
      }
    }

    stage('Archive Artifacts') {
      steps {
        archiveArtifacts artifacts: "${APP_NAME}-${BUILD_NUMBER}.tar", fingerprint: true
      }
    }
  }

  post {
    success {
      echo "✅ Build complete. Docker image saved and archived successfully."
    }
  }
}
