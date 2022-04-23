pipeline {
    environment {
    registry = "manojmadhusoodhanan/nodejs"
    registryCredential = 'DOCKER_HUB_PASSWORD'
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
          steps {
             sh 'docker version'
             sh 'docker build -t $registry .'
             sh 'docker image list'
             sh 'docker tag $registry $registry:latest'
            }
      } 

       stage("Docker Login"){
           steps{
             //withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')])
             //withCredentials([string(credentialsId: 'DOCKERUSER', variable: 'User')]) {
                
               //sh 'docker login -u manojmadhusoodhanan -p $PASSWORD'
               withCredentials([
                   string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD'),
                   string(credentialsId: 'DOCKERUSER', variable: 'USER')]){
               sh 'echo "$PASSWORD" | docker login -u $USER --password-stdin'
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
