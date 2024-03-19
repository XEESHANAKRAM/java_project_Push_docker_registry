pipeline {
  agent any
  tools {
    maven 'maven'
  }
  stages {
    stage('Build Maven') {
      steps {
        script {
          checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/XEESHANAKRAM/java_project_Push_docker_registry']]])
          sh 'mvn clean install'
        }
      }
    }
    stage('Build Docker Image') {
      steps {
        script{
          sh 'docker build -t xeeshanakram/devops-integration .'
        }
      }
    }
    stage('Push Image to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
          sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
          sh 'docker push xeeshanakram/devops-integration'
        }
      }
    }
    stage('Deploy to Minikube') {
      steps {
        script {
          checkout scm
          sh 'kubectl apply -f deploymentservice.yaml --context=minikube'
        }
      }
    }
  }
}
