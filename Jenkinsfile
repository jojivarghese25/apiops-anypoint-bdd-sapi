pipeline {
  agent any
  stages {
    stage('test') {
      steps {
        echo 'kudos'
      }
    }

    stage('build') {
      steps {
        sh 'mvn clean install '
      }
    }

  }
  tools {
    maven 'Maven'
  }
}