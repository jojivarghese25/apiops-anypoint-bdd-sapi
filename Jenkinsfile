pipeline {
  agent any
  stages {
    stage('gitclone') {
      steps {
        git(url: 'https://github.com/Mr-Raviteja/apiops-anypoint-bdd-sapi.git', branch: 'master')
      }
    }

    stage('maven build') {
      steps {
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          bat 'mvn -f apiops-anypoint-bdd-sapi/pom.xml clean install -Dtestfile=runner.TestRunner.java -DskipTests'
        }

      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage= docker.build("ravisunny27/apiops-anypoint-bdd-sapi")
        }

        echo 'image built'
      }
    }

    stage('Run container') {
      steps {
        script {
          bat 'docker run -itd -p 8081:8081 --name apiops-anypoint-bdd-sapi  ravisunny27/apiops-anypoint-bdd-sapi'
        }

        echo 'container running'
      }
    }

    stage('FunctionalTesting') {
      steps {
        
        withEnv(overrides: ["JAVA_HOME=${ tool 'JDK 8' }", "PATH+MAVEN=${tool 'Maven'}/bin:${env.JAVA_HOME}/bin"]) {
          sh 'mvn -f cucumber-API-Framework/pom.xml test -Dtestfile=/src/test/java/runner.TestRunner.java'
        }

      }
    }
    stage('GenerateReports') {
      steps {
        cucumber(failedFeaturesNumber: -1, failedScenariosNumber: -1, failedStepsNumber: -1, fileIncludePattern: 'cucumber.json', jsonReportDirectory: 'cucumber-API-Framework/target', pendingStepsNumber: -1, skippedStepsNumber: -1, sortingMethod: 'ALPHABETICAL', undefinedStepsNumber: -1)
      }
    }
    
    stage('read properties files') {
      steps {
        script {
          readProps= readProperties file: 'cucumber-API-Framework/src/main/resources/email.properties'
        }

        echo "${readProps['email.to']}"
      }
    }

    stage('Email') {
      steps {
        emailext(subject: 'Testing Reports for $PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', body: 'please go to url: $BUILD_URL.'+readFile("cucumber-API-Framework/src/main/resources/emailTemplate.html"), attachmentsPattern: 'cucumber-API-Framework/target/cucumber-reports/report.html', from: "${readProps['email.from']}", mimeType: 'text/html', to: "${readProps['email.to']}", attachLog: true)
      }
    }

    stage('Kill container') {
      steps {
        script {
          bat 'docker rm -f apiops-anypoint-bdd-sapi'
        }

        echo 'container Killed'
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
