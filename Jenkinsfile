pipeline {
  agent any
  stages {
    stage('error') {
      steps {
        script {
          checkout scm
          def customImage = docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }

  }
  environment {
    registry = 'markony/flask-app'
  }
}