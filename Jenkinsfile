pipeline {
  environment {
    registry = "amitsharma6757/my-app"
    registryCredential = 'docker-creds'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
	  git credentialsId: 'git-creds', url: 'https://github.com/amit6757/my-app.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Test my-app' ) {
                agent {
                docker { image 'amitsharma6757/myApp2:$BUILD_NUMBER' }
            }
            steps {
                sh 'my-app --version'
            }
        }


    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}