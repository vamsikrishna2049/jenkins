```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'echo "Welcome to Jenkins Pipeline Declarative Scripting"'
            }
        }
    }
}
```
