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
      switch (env.BRANCH_NAME) {
        case "development":
          sh "sudo docker container run -d --name webserver-development '$registry:${env.GIT_BRANCH}'"
          sh "sudo docker container rm -f webserver-development" clean :app:assembleDebug")
        break
        case "homolog":
          sh "sudo docker container run -d --name webserver-homolog '$registry:${env.GIT_BRANCH}'"
          sh "sudo docker container rm -f webserver-homolog"
        break
        case "production":
          sh "sudo docker container run -d --name webserver-production '$registry:${env.GIT_BRANCH}'"
          sh "sudo docker container rm -f webserver-production"
        break
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
