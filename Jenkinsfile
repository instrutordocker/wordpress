pipeline {
  environment {
    registry = "instrutordocker/wordpress"
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
    stage('Testar imagem Docker') {
      when {
      expression { return env.BRANCH_NAME == 'development'; }}
      steps{
        sh "sudo docker container run -d --name '$registry:development' '$registry:${env.GIT_BRANCH}'"
        sh "sudo docker container rm -f '$registry:development'"
      }
    }
      when {
      expression { return env.BRANCH_NAME == 'homolog'; }}
      steps{
        sh "sudo docker container run -d --name '$registry:homolog' '$registry:${env.GIT_BRANCH}'"
        sh "sudo docker container rm -f '$registry:homolog'"
      }
    }
      when {
      expression { return env.BRANCH_NAME == 'production'; }}
      steps{
        sh "sudo docker container run -d --name '$registry:production' '$registry:${env.GIT_BRANCH}'"
        sh "sudo docker container rm -f '$registry:production'"
      }
    }
    stage('Enviar imagem ao Docker HUB') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "sudo docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
          sh "sudo docker tag '$registry:${env.GIT_BRANCH}' $registry:${env.GIT_BRANCH}"
          sh "sudo docker push $registry:${env.GIT_BRANCH}"
        }
      }
    }
    stage('Remover imagem Docker não utilizada') {
      steps{
        sh "sudo docker rmi '$registry:${env.GIT_BRANCH}'"
      }
    }
  }
}
