pipeline {
  agent any
  stages {
    stage('gitclone') {
      steps {
        git 'https://github.com/Mr-Raviteja/apiops-anypoint-bdd-sapi.git'
      }
    }

    stage('Email') {
      steps {
        emailext(subject: 'Testing Reports', body: 'Please find the functional testing reports.', from: 'test.example.demo123@gmail.com', mimeType: 'text/html', to: 'madishettyraviteja2011@gmail.com')
      }
    }

  }
  tools {
    maven 'Maven'
  }
  post {
    failure {
      emailext(subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', body: 'Please find attached logs.', attachLog: true, from: 'test.example.demo123@gmail.com', to: 'raviteja.madishetty@njclabs.com')
    }

  }
}