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
    stage('SonarQube analysis') {
    environment {
      SCANNER_HOME = tool 'sonar-scanner'
    }
    steps {
    withSonarQubeEnv(credentialsId: 'sonar-auth', installationName: 'sonar') {
         sh '''$SCANNER_HOME/bin/sonar-scanner \
         -Dsonar.projectKey=projectKey \
         -Dsonar.projectName=projectName \
         -Dsonar.sources=src/ \
         -Dsonar.java.binaries=target/classes/ \
         -Dsonar.exclusions=src/test/java/****/*.java \
         -Dsonar.java.libraries=/root/.m2/**/*.jar \
         -Dsonar.projectVersion=${BUILD_NUMBER}'''
       }
     }
    }
    stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
         }
    }
      
    stage('Deploy to Cluster') {
          steps {
                    sh 'envsubst < ${WORKSPACE}/config_server_deployment.yml | kubectl apply -f -'
          }
      }
  }
}
