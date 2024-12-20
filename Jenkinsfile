pipeline {
    agent any

    environment {
        ARTIFACT_NAME = 'my-artifact.war'  // Ensure artifact name matches the expected output
    }
    
    tools {
        maven 'maven-3.9.7'  // Ensure you have Maven configured on Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Cloning repository...'
                    git url: 'https://github.com/vamsikrishna2049/jenkins.git', branch: 'master', credentialsId: 'your-credentials-id'
                }
                sh 'git status'  // Validate git repository status
            }
        }

        stage('Build') {
            steps {
                echo 'Starting the build process...'
                sh 'mvn clean package'

                script {
                    // Validate that the artifact was created
                    def artifactExists = fileExists("target/${ARTIFACT_NAME}")
                    if (!artifactExists) {
                        error "Build artifact not found: ${ARTIFACT_NAME}!"
                    }
                }
            }
        }

        stage('Sonar Analysis') {
            steps {
                script {
                    echo 'Running SonarQube analysis...'
                    // Run Sonar analysis
                    sh 'mvn sonar:sonar -Dsonar.projectKey=my_project_key -Dsonar.host.url=http://your-sonar-url'
                }
            }
        }

        stage('Deploy to Dev Server') {
            steps {
                sshagent([SSH_KEY_ID]) {
                    script {
                        echo "Transferring artifact to the server..."
                        sh """
                        scp target/${ARTIFACT_NAME} ${DEV_SERVER_USER}@${DEV_SERVER_IP}:${DEV_SERVER_PATH}/
                        """
                        // Validate the artifact file is on the server
                        sh """
                        ssh ${DEV_SERVER_USER}@${DEV_SERVER_IP} 'ls -lh ${DEV_SERVER_PATH}/${ARTIFACT_NAME}'
                        """
                    }
                }
            }
        }

        stage('Restart Application') {
            steps {
                sshagent([SSH_KEY_ID]) {
                    script {
                        // Check if the service is running before attempting to restart it
                        def status = sh(script: "ssh ${DEV_SERVER_USER}@${DEV_SERVER_IP} 'sudo systemctl is-active your-app-service'", returnStatus: true)
                        if (status != 0) {
                            error "The application service is not active. Cannot restart."
                        }

                        echo "Restarting the application..."
                        sh """
                        ssh ${DEV_SERVER_USER}@${DEV_SERVER_IP} 'sudo systemctl restart your-app-service'
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            script {
                // Send Slack success notification
                slackSend(
                    channel: '#all-devops-practise', // Replace with your Slack channel
                    color: 'good', // Green color for success
                    message: "Build Successful: ${env.JOB_NAME} (${env.BUILD_NUMBER}) - Deployed to ${params.select_env}."
                )
            }
            archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
        }
        failure {
            script {
                // Send Slack failure notification
                slackSend(
                    channel: '#all-devops-practise', // Replace with your Slack channel
                    color: 'danger', // Red color for failure
                    message: "Build Failed: ${env.JOB_NAME} (${env.BUILD_NUMBER}) - Please check the build logs."
                )
            }
            archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
        }
        unstable {
            script {
                // Send Slack unstable notification
                slackSend(
                    channel: '#all-devops-practise', // Replace with your Slack channel
                    color: 'warning', // Yellow color for unstable builds
                    message: "Build Unstable: ${env.JOB_NAME} (${env.BUILD_NUMBER}) - Something went wrong."
                )
            }
            archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
        }
    }
}
