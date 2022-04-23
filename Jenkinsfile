pipeline {
    environment {
    registry = "manojmadhusoodhanan/nodejs"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }

  agent any

  stages {
      stage('Git_Clone') {
        steps {
         git credentialsId: 'Github', url: 'https://github.com/manojmadhusoodhanan/NodeJs-App.git'
      }
    }
    
      stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t $registry .'
        sh 'docker image list'
        sh 'docker tag $registry:nodelatest'
    } 
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                    }
                }
            }
        }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "k8s")
        }
      }
    }

  }
}
