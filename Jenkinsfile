pipeline {
    // Define the agent where the pipeline will run
    agent {
        label 'DevServer'  // Specify the Jenkins node/agent labeled 'DevServer'
    }

    // Parameters: Define parameters that can be provided when triggering the build
    parameters {
        string defaultValue: 'Krishna', name: 'LastName'  // Define a string parameter 'LastName' with a default value 'Krishna'
    }

    // Environment: Set environment variables that will be available throughout the pipeline
    environment {
        NAME = "vamsi"  // Define an environment variable 'NAME' with the value 'vamsi'
    }

    // Tools: Define tools required for the pipeline, e.g., Maven
    tools {
        maven 'maven-3.9.7'  // Use the Maven tool with version 'maven-3.9.7' (configured in Jenkins)
    }

    // Stages: Define the stages of the pipeline
    stages {
        // 'Build' stage where the Maven build is executed
        stage('Build') {
            steps {
                // Execute the Maven command to clean and package the project
                sh 'mvn clean package'  // Run Maven clean and package commands to build the WAR file

                // Print a message with the environment variable 'NAME'
                sh 'echo "started by $NAME"'  // This will print 'started by vamsi'
            }

            // Post-actions to be executed after the stage completes
            post {
                // If the 'Build' stage is successful, this block is executed
                success {
                    // Archive the generated WAR files
                    archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true  // Archives any WAR files found in the target/ directory
                }
            }
        }
    }
}
