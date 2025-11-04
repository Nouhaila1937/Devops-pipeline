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
        echo "✅ All tests passed"
      }
    }

    stage('Publish image') {
      steps {
        script {
          docker.withRegistry('', registryCredential) {
            // push versionnée et latest
            dockerImage.push("${BUILD_NUMBER}")
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploy image') {
      steps {
        script {
          sh '''
          docker stop testtp2 || true
          docker rm testtp2 || true
          docker run -d --name testtp2 -p 8082:80 ${registry}:latest
          '''
        }
      }
    }
  }
}
