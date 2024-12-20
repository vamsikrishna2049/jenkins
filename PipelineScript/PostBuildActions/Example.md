Regardless of the outcome of the pipeline stage, post build actions that should be executed. 
Whether the build was successful, failed, or unstable, the actions in the always block will always run.

# Typical Use Cases for always:
**1. Clean-up Tasks:**
You might want to clean up temporary files, remove Docker containers, or perform other cleanup tasks after the pipeline finishes, no matter the result.

**2. Notifications:**
Sending notifications (such as an email or Slack message) that a pipeline has finished, regardless of success or failure.

**3. Archiving Logs:**
You may want to always archive certain logs or artifacts, even if the build fails, for troubleshooting purposes.

**4. Resource Management:**
Stopping or cleaning up infrastructure resources like virtual machines, containers, or cloud instances that were spun up during the pipeline execution.

```groovy
post {
    success {
        // Actions to take if the build is successful
        echo 'Build was successful!'
        archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
    }
    failure {
        // Actions to take if the build fails
        echo 'Build failed!'
        // You could notify team members, e.g., sending an email or a Slack message
    }
    unstable {
        // Actions to take if the build is unstable
        echo 'Build is unstable.'
    }
    always {
        // Actions to take no matter what the outcome is
        echo 'This will always run regardless of the build result.'
        
        // Clean up resources (e.g., remove temp files, stop Docker containers)
        deleteDir()  // This cleans up the workspace by deleting all files
        
        // Optionally, you can also send a completion notification
        // sendNotification('Pipeline has finished.')
    }
}
```

---

***Example:
// Post-actions to be executed after the stage completes
post {
    // If the 'Build' stage is successful, this block is executed
    success {
        // Archive the generated WAR files
        archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true  // Archives any WAR files found in the target/ directory
    }
}
