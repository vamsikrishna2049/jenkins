pipeline {
  agent {
    label 'DevServer'
  }

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    post {
      success {
        archiveArtifacts artifacts: 'target/*.war'
      }
    }
  }
}
