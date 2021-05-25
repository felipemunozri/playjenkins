pipeline {

  environment {
    registry = 'felipemunozri/playjenkins'
    registryCredential = 'dockerhub_id'
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/felipemunozri/playjenkins.git'
        // checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/felipemunozri/playjenkins.git']]])
      }
    }

    stage('Build image') {
      steps{
        script {
          // dockerImage = docker.build registry + ":$BUILD_NUMBER"
          dockerImage = docker.build registry + ":latest"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "autocluster_config")
        }
      }
    }

  }

}