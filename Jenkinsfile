pipeline {

  environment {
    dockerimagename = "shiva9921/hello-world-python"
    dockerImage = "hello-world-python"
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/shivam779823/jenkins-project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

  

  }

}