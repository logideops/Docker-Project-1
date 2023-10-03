pipeline {

  environment {
    registry = "github.com/logideops/Docker-Project-1/flask"
    registry_mysql = "github.com/logideops/Docker-Project-1/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/logideops/Docker-Project-1.git'
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
       sh 'docker build -t "github.com/logideops/Docker-Project-1/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "hub.docker.com/repository/docker/logidevops/testproject_1/mysql:$BUILD_NUMBER"'
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
