pipeline {
  agent { labbel "dev" }
  stages {
    stage("Code") {
      steps {
        git url: "https://github.com/iamamash/Django-Notes-App.git", branch: "main"
      }
    }
    stage("Build and Test") {
      steps {
        sh "docker build . -t webapp"
      }
    }
    stage("Login and Push") {
      steps {
        withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"password",usernameVariable:"user")]) {
          sh "docker login -u ${env.user} -p ${env.password}"
          sh "docker tag webapp ${env.user}/django-notes-app:latest"
          sh "docker push ${env.user}/django-notes-app:latest"
        }
      }
    }
    stage("Deploy") {
      steps {
        sh "docker-compose down && docker-compose up -d"
      }
    }
  }
}
