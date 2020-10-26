pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml clean install -Dtestfile=runner.TestRunner.java'
        }

      }
    }

    stage('Deploy') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml package deploy -DmuleDeploy -Dtestfile=runner.TestRunner.java -Danypoint.username=vikaspuri5 -Danypoint.password=Mihir@14puri -DapplicationName=apiops-bdd-sapi -Dcloudhub.region=us-east-2'
        }

      }
    }

    stage('IntegerationTesting') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          sh 'mvn -f apiops-anypoint-bdd-sapi/pom.xml test -Dtestfile=hello.java'
        }

      }
    }

    stage('Email') {
      steps {
        emailext(subject: 'Testing Reports for $PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', body: 'Please find the functional testing reports. In order to check the logs also, please go to url: $BUILD_URL', attachmentsPattern: 'apiops-anypoint-bdd-sapi/target/cucumber-reports/report.html', from: 'testmailsnjc@gmail.com', mimeType: 'text/html', to: 'vikas_mullana@yahoo.com')
      }
    }

  }
  tools {
    maven 'Maven'
  }
  post {
    failure {
      emailext(subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', body: 'Please find attached logs.', attachLog: true, from: 'testmailsnjc@gmail.com', to: 'vikas_mullana@yahoo.com')
    }

  }
}