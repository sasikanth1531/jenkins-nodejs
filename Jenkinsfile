pipeline {
  environment {
    dockerimagename = "sasikanth777/nodejs"
    dockerImage = "node"
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        // Checkout the source code from the GitHub repository
        git url: 'https://github.com/sasikanth1531/jenkins-nodejs.git'
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

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }
}
