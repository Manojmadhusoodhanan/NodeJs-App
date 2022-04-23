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
          steps {
             sh 'docker version'
             sh 'docker build -t $registry .'
             sh 'docker image list'
             sh 'docker tag $registry $registry:latest'
            }
      } 

      stage("Docker Login"){
        withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
            sh 'docker login -u manojpillai.mail -p $PASSWORD'
        }
    } 

      stage("Push image") {
            steps {
                script {
                    docker push $registry:latest
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
