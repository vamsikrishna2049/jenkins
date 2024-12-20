### **Key Components of a Jenkins Pipeline**
---
1. **Agent**: Defines where the pipeline will run (e.g., on which Jenkins node or machine).
2. **Stages**: Represents different phases of the pipeline (e.g., Build, Test, Deploy).
3. **Steps**: Commands or actions that run within each stage (e.g., run a script, execute a shell command).
4. **Post**: Defines actions to take after the stages (e.g., cleanup, notification, archiving).
5. **Environment**: Defines environment variables that can be used throughout the pipeline.
6. **Parameters**: Defines input values (e.g., user input) for the pipeline at the time of triggering the job.

### **Basic Structure of a Declarative Pipeline**

Here’s the basic structure of a **Declarative Pipeline**:
```groovy
pipeline {
    agent any  // Defines where the pipeline will run (e.g., any available agent)
    
    environment {
        // Define environment variables available to the entire pipeline
        MY_ENV_VAR = 'value'
    }

    parameters {
        // Define parameters (inputs) for the pipeline
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
    }

    stages {
        // Define the stages of the pipeline
        stage('Build') {
            steps {
                // Commands or actions to run in this stage
                echo 'Building the application...'
                sh 'mvn clean package'  // Example of running a Maven command
            }
        }
        
        stage('Test') {
            steps {
                // Commands or actions for testing
                echo 'Running tests...'
                sh 'mvn test'
            }
        }
        
        stage('Deploy') {
            steps {
                // Commands or actions for deployment
                echo 'Deploying application...'
                sh './deploy.sh'
            }
        }
    }

    post {
        // Actions to run after all stages have completed
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
        always {
            // Actions that always run (e.g., clean up)
            echo 'Cleaning up...'
        }
    }
}
```

### **Detailed Breakdown of Pipeline Script Components**

1. **Pipeline Block**: 
   - The entire pipeline is wrapped inside the `pipeline { ... }` block.
   - This is mandatory in a **Declarative Pipeline**.

2. **Agent**:
   - Defines where the pipeline will run. It could be a specific node, a Docker container, or any available agent.
   - Commonly used values for the agent include:
     - `any`: Runs the pipeline on any available agent.
     - `label 'node_label'`: Runs the pipeline on a specific node with that label.
     - `docker 'image_name'`: Runs the pipeline in a Docker container using the specified image.

   ```groovy
   agent any
   ```

3. **Environment Variables**:
   - Use the `environment { ... }` block to define global environment variables that can be accessed across all stages.
   - Example:
     ```groovy
     environment {
         MY_VAR = 'some_value'
     }
     ```

4. **Parameters**:
   - The `parameters` block allows you to define input parameters for your pipeline (e.g., branch names, version numbers).
   - Example:
     ```groovy
     parameters {
         string(name: 'BRANCH', defaultValue: 'main', description: 'Git branch to build')
     }
     ```

5. **Stages**:
   - A pipeline consists of multiple `stages` that represent different phases of the process (e.g., build, test, deploy).
   - Each stage has its own `steps` block, which contains the commands to be executed in that stage.
   - Example of a build stage:
     ```groovy
     stage('Build') {
         steps {
             echo 'Building application...'
             sh 'mvn clean install'
         }
     }
     ```

6. **Post Actions**:
   - The `post { ... }` block contains actions that will be executed after the pipeline completes, regardless of whether it succeeded or failed.
   - You can define actions for different outcomes, such as `success`, `failure`, or `always`.
   - Example:
     ```groovy
     post {
         success {
             echo 'Build was successful!'
         }
         failure {
             echo 'Build failed. Please check the logs.'
         }
         always {
             echo 'This will always run, regardless of the build result.'
         }
     }
     ```

### **Example: A Complete Declarative Pipeline Script**

```groovy
pipeline {
    agent any  // Use any available agent to run the pipeline

    environment {
        BUILD_VERSION = '1.0.0'  // Global environment variable
    }

    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
    }

    stages {
        stage('Build') {
            steps {
                echo "Building version ${BUILD_VERSION} from branch ${params.BRANCH}"
                sh 'git checkout ${params.BRANCH}'  // Checkout the branch defined by the user
                sh 'mvn clean package'  // Build the application using Maven
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'  // Run unit tests
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to server...'
                sh './deploy.sh'  // Run deployment script
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please review the logs.'
        }
        always {
            echo 'Cleaning up resources...'
        }
    }
}
```

### **Advanced Features in Jenkins Pipeline**

1. **Parallel Stages**:
   - You can run multiple stages concurrently using the `parallel` block. This is especially useful for tasks like running tests in parallel across multiple environments or performing independent builds.
   ```groovy
   stage('Test') {
       parallel {
           stage('Unit Tests') {
               steps {
                   sh 'mvn test -Dtest=UnitTests'
               }
           }
           stage('Integration Tests') {
               steps {
                   sh 'mvn test -Dtest=IntegrationTests'
               }
           }
       }
   }
   ```

2. **Retry Logic**:
   - You can add retry logic to handle intermittent failures.
   ```groovy
   stage('Test') {
       steps {
           retry(3) {
               sh 'mvn test'
           }
       }
   }
   ```

3. **Timeouts**:
   - Set a timeout for stages to avoid getting stuck in case of failures or long-running tasks.
   ```groovy
   stage('Deploy') {
       steps {
           timeout(time: 30, unit: 'MINUTES') {
               sh './deploy.sh'
           }
       }
   }
   ```

4. **Conditional Execution**:
   - You can use conditions to control whether a stage runs based on the outcome of previous stages.
   ```groovy
   stage('Deploy') {
       when {
           branch 'main'  // Only deploy on the 'main' branch
       }
       steps {
           sh './deploy.sh'
       }
   }
   ```

5. **Shared Libraries**:
   - If you have common pipeline steps across multiple projects, you can create shared libraries for reuse.
   ```groovy
   @Library('my-shared-library') _
   ```

---
