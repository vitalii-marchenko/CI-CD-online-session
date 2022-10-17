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
            docker.image("${registry}:${env.BUILD_ID}").inside { c ->
            sh 'python app_test.py'
        }
      }
    }
  }
    
    stage('Http Test') {
      steps {
        script {
          docker.image("${registry}:${env.BUILD_ID}").withRun('-p 9005:9000') { c ->
          sh "sleep 5; curl -i http://localhost:9005/test_string"
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
