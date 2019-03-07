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
        sh "sudo docker build . -t '$registry:${BUILD_NUMBER}'"
      }
    }
    stage('Testar imagem Docker') {
      steps{
        sh "sudo docker container run -d --name '$registry:${BUILD_NUMBER}' '$registry:$BUILD_NUMBER'"
        sh "sudo docker container rm -f '$registry:${BUILD_NUMBER}'"
      }
    }
    stage('Enviar imagem ao Docker HUB') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "sudo docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
          sh "sudo docker tag '$registry:${BUILD_NUMBER}' $registry:${env.GIT_BRANCH}"
          sh "sudo docker push $registry:${env.GIT_BRANCH}"
        }
      }
    }
    stage('Remover imagem Docker não utilizada') {
      steps{
        sh "sudo docker rmi '$registry:${BUILD_NUMBER}'"
      }
    }
  }
}
