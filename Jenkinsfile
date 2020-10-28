pipeline {
  agent any
  stages {
    stage('hello') {
      steps {
        echo 'Hello'
      }
    }

    stage('email') {
      steps {
        emailext(body: 'Hello', subject: 'Jenkins', to: 'madishettyraviteja2011@gmail.com')
      }
    }

  }
  tools {
    maven 'Maven'
  }
  post {
    always {
      emailext(subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', body: 'Please find attached logs.', from: 'test.example.demo123@gmail.com', to: 'raviteja.madishetty@njclabs.com')
    }

  }
}