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
            // withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')])
             //withCredentials([file(credentialsId: 'DOCKERUSER', variable: 'User')]) {
                
               //sh 'docker login -u manojmadhusoodhanan -p $PASSWORD'
               withCredentials([
                   usernamePassword(credentialsId: credsId1, usernameVariable: 'DOCKER_HUB_PASSWORD', passwordVariable: 'PASSWORD'),
                   usernamePassword(credentialsId: credsId2, usernameVariable: 'DOCKERUSER', passwordVariable: 'User')]){
               sh 'echo "$PASSWORD" | docker login -u $User --password-stdin'
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
