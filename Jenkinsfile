pipeline {
    environment {
    registry = "manojmadhusoodhanan/nodejs"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }

  agent any

  stages {

      stage('clone') {
       git credentialsId: 'Github', url: 'https://github.com/rahulwagh/spring-boot-docker.git'
     }

      stage('Git_Clone') {
      steps {
        git credentialsId: 'Github', url: 'https://github.com/manojmadhusoodhanan/NodeJs-App.git'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                  dockerImage = docker.build registry + ":latest"
                }
            }
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
