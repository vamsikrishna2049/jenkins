pipeline {
  agent any

  parameters {
    choice(name: 'select_env', choices: ['dev', 'staging', 'prod'], description: 'Choose the environment for deployment')
  }

  tools {
    maven 'maven-3.9.7'  // Make sure Maven is correctly defined for your agent
  }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/vamsikrishna2049/jenkins.git', branch: 'master'
      }
    }

    stage('Build') {
      steps {
        echo 'Starts Building the Artifacts'
        sh 'mvn clean package'
      }
    }

    stage('Sonar Analysis') {
      steps {
        script {
          // Ensure SonarQube environment variables are set (you may need to configure SonarQube server details in Jenkins)
          sh 'mvn sonar:sonar'
        }
      }
    }
  }

  post {
    success {
      script {
        // Send Slack success notification
        slackSend(
          channel: '#all-devops-practise',  // Replace with your Slack channel
          color: 'good',                    // Green color for success
          message: "Build Successful: ${env.JOB_NAME} (${env.BUILD_NUMBER}) - Deployed to ${params.select_env}."
        )
      }
      archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
    }
    failure {
      script {
        // Send Slack failure notification
        slackSend(
          channel: '#all-devops-practise',  // Replace with your Slack channel
          color: 'danger',                  // Red color for failure
          message: "Build Failed: ${env.JOB_NAME} (${env.BUILD_NUMBER}) - Please check the build logs."
        )
      }
      archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
    }
    unstable {
      script {
        // Send Slack unstable notification
        slackSend(
          channel: '#all-devops-practise',  // Replace with your Slack channel
          color: 'warning',                 // Yellow color for unstable builds
          message: "Build Unstable: ${env.JOB_NAME} (${env.BUILD_NUMBER}) - Something went wrong."
        )
      }
      archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
    }
  }
}
