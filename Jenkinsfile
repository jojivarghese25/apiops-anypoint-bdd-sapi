pipeline {
  agent any
  stages {
    stage('gitclone') {
      steps {
        git 'https://github.com/Mr-Raviteja/apiops-anypoint-bdd-sapi.git'
      }
    }

    stage('Build') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          bat 'mvn -f apiops-anypoint-bdd-sapi/pom.xml clean install -Dtestfile=runner.TestRunner.java'
        }

      }
    }

    stage('Deploy') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          bat 'mvn -f apiops-anypoint-bdd-sapi/pom.xml package deploy -DmuleDeploy -Dtestfile=runner.TestRunner.java -Danypoint.username=Mr_Raviteja4 -Danypoint.password=Madishetty@27 -DapplicationName=apiops-bdd-sapi -Dcloudhub.region=us-east-2'
        }

      }
    }

    stage('IntegerationTesting') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          bat 'mvn -f apiops-anypoint-bdd-sapi/pom.xml test -Dtestfile=hello.java'
        }

      }
    }

  }
  tools {
    maven 'Maven'
  }
}