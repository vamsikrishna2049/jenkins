pipeline {
  agent {
    label 'DevServer'
  }

  parameters {
    string defaultValue: 'Krishna', name: 'LastName'
  }

  environment {
    NAME = "vamsi"
  }

  tools {
    maven 'maven-3.9.7'
  }

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
        sh 'started by $NAME'
      }

      post {
        success {
          archiveArtifacts artifacts: 'target/*.war'
        }
      }
    }
  }
}
