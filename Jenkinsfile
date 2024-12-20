pipeline {
  agent {
    label 'DevServer'
  }

  tools {
    maven 'maven-3.9.7'
  }

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    post {
      success {
        archiveArtifacts artifacts: 'target/*.jar'
      }
    }

  }
}
