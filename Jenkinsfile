pipeline {
    environment {
    registry = "manojmadhusoodhanan/nodejs"
    tag = "latest"
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
             sh 'docker rmi -f $(docker images -aq)'
             sh 'docker build -t $registry .'
             sh 'docker image list'
             sh 'docker tag $registry $registry:$tag'
            }
      } 

       stage("Docker Login"){
           steps{
               withCredentials([
                   string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD'),
                   string(credentialsId: 'DOCKERUSER', variable: 'USER')]){
               sh 'echo "$PASSWORD" | docker login -u $USER --password-stdin'
           } 
        } 
     }


       stage('Docker push') {
           steps {
              sh  'docker push $registry:$tag'
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
