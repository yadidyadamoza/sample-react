pipeline {

  environment {
    dockerimagename = "h264/sample-react"
    dockerImage = "h264/sample-react:latest"
  }


  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/pandareen/sample-react/'
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
               registryCredential = 'dockerhub'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
      }
    }

  }

}
