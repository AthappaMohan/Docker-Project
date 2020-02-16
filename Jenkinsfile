pipeline {

  environment {
    registry = "10.172.0.2:5000/athappamohan/flask"
    registry_mysql = "10.172.0.2:5000/athappamohan/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source-supercubensquare') {
      steps {
        git 'https://github.com/athappamohan/Docker-Project.git'
      }
    }

    stage('Build image-supercubensquare') {
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
       sh 'docker build -t "10.172.0.2:5000/athappamohan/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "10.172.0.2:5000/athappamohan/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
