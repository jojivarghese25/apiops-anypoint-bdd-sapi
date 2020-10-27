pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml clean install -Dtestfile=runner.TestRunner.java'
      }
    }

    stage('test') {
      steps {
        echo 'kudos'
      }
    }

  }
  tools {
    maven 'Maven'
  }
}