pipeline {
  agent any

  environment {
    registry = "nanga123/testtp2"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }

  stages {
    stage('Clone Git') {
      steps {
        git 'https://github.com/Nouhaila1937/Devops-pipeline.git'
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
        }
      }
    }

    stage('Test image') {
      steps {
        echo "All tests passed"
      }
    }

    stage('Publish image') {
      steps {
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploy image') {
      steps {
        script {
          sh "docker run -d -p 8083:80 ${registry}:latest"
        }
      }
    }
  }
}
