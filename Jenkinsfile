pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn compile'
      }
    }

    stage('Analysis') {
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh 'mvn clean package && mvn test -Dtest=CalculatorSpec && mvn site'
        }
      }
    }
  }
  post {
      always {
        junit testResults: '**/target/surefire-reports/TEST-*.xml'

        recordIssues enabledForFailure: true, tool: spotBugs()
      }
  }
}