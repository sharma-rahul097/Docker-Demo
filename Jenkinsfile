pipeline {
  tools {
    maven 'maven'
  }
  environment {
     REPOSITORY_TAG="rshar123/parivesh2_dev:${BUILD_NUMBER}"
   }
  agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sharma-rahul097/Docker-Demo.git'
            }
        }
        
    stage('Maven build') {
          steps {  
     	      script { 
               sh 'mvn install'
            }
          }
        }

    stage('Build and Push Docker Image') {
      environment {
        REGISTRY_CREDENTIALS = credentials('docker-cred')
      }
      steps {
        script {
            sh 'docker build -t ${REPOSITORY_TAG} .'
            def dockerImage = docker.image("${REPOSITORY_TAG}")
            docker.withRegistry('https://registry.hub.docker.com', "docker-cred") {
                dockerImage.push()
            }
        }
      }
    }
    stage('Deploy to Cluster') {
          steps {
                    sh 'envsubst < ${WORKSPACE}/config_server_deployment.yaml | kubectl apply -f -'
          }
      }
  }
}
