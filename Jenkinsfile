pipeline {
  tools {
    maven 'maven'
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
        DOCKER_IMAGE = "rshar123/parivesh2_dev:${BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = credentials('docker-cred')
      }
      steps {
        script {
            sh 'docker build -t ${DOCKER_IMAGE} .'
            def dockerImage = docker.image("${DOCKER_IMAGE}")
            docker.withRegistry('https://registry.hub.docker.com', "docker-cred") {
                dockerImage.push()
            }
        }
      }
    }
    stage('Deploying parivesh-backend container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "config_server_deployment.yml", "config_server_service.yml")
        }
      }
    }
  }
}
