pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn compile'
      }
    }

    stage('Test') {
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh 'mvn test -Dtest=CalculatorSpec'
        }
      }
    }

    stage ('Analysis') {
      steps {
        sh 'mvn site'
      }
    }

    post {
      always {
        junit testResults: '**/target/surefire-reports/TEST-*.xml'

        recordIssues enabledForFailure: true, tool: findbugs(pattern: '**/target/site/cpd.xml')
      }
    }

  }
}