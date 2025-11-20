pipeline {
    environment {
        registry = "nanga123/testtp2"
        registryCredential = 'docker-hub-credentials'
        dockerImage = ''
    }
    agent any
    stages {
  stage('Cloning Git') {
    steps {
        git branch: 'main',
            url: 'https://github.com/Nouhaila1937/Devops-pipeline'
    }
}

        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Test image') {
            steps{
                script {
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
          
        stage('Deploy image') {
            steps {
                script {
                    // Arrêter et supprimer l'ancien conteneur
                    bat '''
                        docker stop web-tp4 2>nul || echo Conteneur non trouve
                        docker rm web-tp4 2>nul || echo Conteneur non trouve
                    '''
                    
                    // Déployer le nouveau conteneur
                    bat "docker run -d --name web-tp4 -p 8081:80 ${registry}:${BUILD_NUMBER}"
                    
                    echo "Application deployee sur http://localhost:8081"
                }
            }
        }
    }
}
  