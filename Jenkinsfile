pipeline {
  environment {
    dockerImageName = "sasikanth777/nodejs" // Changed variable name to follow camelCase convention
    registryCredential = 'dockerhublogin' // Moved registryCredential to the top-level environment block for consistency
  }

  agent any

  stages {
    stage('Checkout Source') {
      steps {
        // Checkout the source code from the GitHub repository
        git url: 'https://github.com/sasikanth1531/jenkins-nodejs.git'
      }
    }

    stage('Build Image') {
      steps {
        script {
          // Building Docker image
          dockerImage = docker.build dockerImageName
        }
      }
    }

    stage('Pushing Image') {
      steps {
        script {
          // Pushing Docker image to Docker Hub registry
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          // Deploying application to Kubernetes
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }
  }
}
