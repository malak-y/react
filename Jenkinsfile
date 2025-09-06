pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/malak-y/react'
      }
    }

    stage('Build Docker image') {
      steps {
        sh '''
          docker build -t react-app .
        '''
      }
    }

    stage('Run Container') {
      steps {
        sh '''
          docker stop react-container || true
          docker rm react-container || true
          docker run -d -p 8080:80 --name react-container react-app
        '''
      }
    }
  }
}
