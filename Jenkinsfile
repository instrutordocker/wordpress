pipeline {
  environment {
    registry = "instrutordocker/wordpress-devops"
    registryCredential = 'dockerhub'
  }
  agent any
  stages {
    stage('Clonar Repositório do GitLab') {
      steps {
        git 'https://github.com/instrutordocker/wordpress.git'
      }
    }
    stage('Construir imagem Docker') {
      steps{
        sh "sudo docker build . -t '$registry:${env.GIT_BRANCH}'"
      }
    }
    stage('Testar imagem Docker'){
      steps{
          sh "sudo docker container run -d --name 'webserver-${env.GIT_BRANCH}' '$registry:${env.GIT_BRANCH}'"
          sh "sudo docker container rm -f 'webserver-${env.GIT_BRANCH}'"
    }
  }
  }
}
