pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm
          def customImage = docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }
    
        stage('Unit tests') {
      steps {
        script {
          docker.image("${registry}:${env.BUILD_ID}").inside {c ->
          sh 'python app_test.py'
        }
      }

    }
  }

    stage('Push to Registry') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub_id') {
            docker.image("${registry}:${env.BUILD_ID}").push('latest')
            docker.image("${registry}:${env.BUILD_ID}").push("${env.BUILD_ID}")
          }
        }

      }
    }

  }
  environment {
    registry = 'markony/flask-app'
  }
}
