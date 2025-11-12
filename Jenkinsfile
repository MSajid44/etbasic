pipeline {
  agent any

  environment {
    REPO_URL  = 'https://github.com/MSajid44/Expense-Tracker.git'
    APP_NAME  = 'expense-tracker'
    DOCKER_USER = 'muhammadsajid44'
    IMAGE_TAG = "${DOCKER_USER}/${APP_NAME}:${BUILD_NUMBER}"
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

    stage('Login to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
          sh '''
            echo "Logging in to Docker Hub..."
            echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
          '''
        }
      }
    }

    stage('Push Image to Docker Hub') {
      steps {
        sh '''
          echo "Pushing image to Docker Hub..."
          docker push ${IMAGE_TAG}
        '''
      }
    }
  }

  post {
    success {
      echo "✅ Image successfully pushed to Docker Hub: ${IMAGE_TAG}"
    }
  }
}
