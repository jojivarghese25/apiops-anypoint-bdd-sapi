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
        sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml clean install'
      }
    }

  }
  tools {
    maven 'Maven'
  }
}