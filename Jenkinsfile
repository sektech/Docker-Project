pipeline {

  environment {
    registry = "https://hub.docker.com/repositories/sektech1484/flask"
    registry_mysql = "https://hub.docker.com/repository/docker/sektech1484/mysql"
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
          docker.withRegistry( "" ) {
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
       sh 'docker build -t "https://hub.docker.com/repository/docker/sektech1484/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "https://hub.docker.com/repository/docker/sektech1484/mysql:$BUILD_NUMBER"'
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
