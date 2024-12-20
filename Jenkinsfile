pipeline {
  // Define the agent where the pipeline will run
  agent {
    label 'DevServer' // Specify the Jenkins node/agent labeled 'DevServer'
  }

  // Parameters: Define parameters that can be provided when triggering the build
  parameters {
    // Define a string parameter 'LastName' with a default value 'Krishna'
      choice choices: ['dev', 'prod'], name: 'select_env'
  }

  // Environment: Set environment variables that will be available throughout the pipeline
  environment {
    // Define an environment variable 'NAME' with the value 'vamsi'
    NAME = "vamsi"
  }

  // Tools: Define tools required for the pipeline, e.g., Maven
  tools {
    // Use the Maven tool with version 'maven-3.9.7' (configured in Jenkins)
    maven 'maven-3.9.7'
  }

  // Stages: Define the stages of the pipeline
  stages {
    
    // 'Build' stage where the Maven build is executed
    stage('Build') {
      steps {
        // Run Maven clean and package commands to build the WAR file
        sh 'mvn clean package'
      }

      // Post-actions to be executed after the stage completes
      post {
        // If the 'Build' stage is successful, this block is executed
        success {
          // Archive the generated WAR files
          archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
          // Archives any WAR files found in the target/ directory, even if none exist
        }
      }
    }

    // 'Test' stage with parallel execution of sub-stages
    stage('Test') {
      parallel {
        // Sub-stage 'test A' - can contain test logic for A
        stage('Test A') {
          steps {
            echo 'This is stage A' // Prints a message indicating stage A execution
          }
        }
        
        // Sub-stage 'test B' - can contain test logic for B
        stage('Test B') {
          steps {
            echo 'This is stage B' // Prints a message indicating stage B execution
          }
        }
      }
    }
  }
}
