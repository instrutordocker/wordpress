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
        sh "sudo docker build . -t '$registry:${env.GIT_BRANCH}-temp'"
      }
    }
    stage('Testar imagem Docker'){
      steps{
          sh "sudo docker container run -d --name 'webserver-${env.GIT_BRANCH}' '$registry:${env.GIT_BRANCH}-temp'"
          sh "sudo docker container rm -f 'webserver-${env.GIT_BRANCH}'"
    }
  }
    stage('Enviar imagem ao Docker HUB') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "sudo docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
          sh "sudo docker tag '$registry:${env.GIT_BRANCH}-temp' $registry:${env.GIT_BRANCH}"
          sh "sudo docker push $registry:${env.GIT_BRANCH}"
        }
      }
    }
    stage('Remover imagem Docker temporária') {
      steps{
        sh "sudo docker rmi -f '$registry:${env.GIT_BRANCH}-temp'"
      }
    }
  }
}
