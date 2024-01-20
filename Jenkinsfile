pipeline {

  environment {
    registry = "sektech1484/flask1"
    registry_mysql = "sektech1484/mysql1"
    registryCredential="docker"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/mgsgoms/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "",registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
  stage('Build mysql image') {
      steps{
        script {
          dockerImage = docker.build registry_mysql + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push mysql Image') {
      steps{
        script {
          docker.withRegistry( "",registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
