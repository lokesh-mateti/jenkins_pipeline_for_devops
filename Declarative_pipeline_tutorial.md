## Jenkins Declarative Pipelines: A Comprehensive Tutorial

### What is Jenkins?

Jenkins is an open-source automation server that helps automate parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery (CI/CD).

### Why Use Jenkins Declarative Pipelines?

Declarative Pipelines provide a simplified and more structured way to define your Jenkins pipelines. They allow you to write your pipeline as code, making it easier to manage, version, and review. Declarative syntax offers a more straightforward and less error-prone way to create pipelines compared to Scripted Pipelines.

### Key Benefits of Declarative Pipelines:
- **Readability**: Easier for humans to read and understand.
- **Structure**: Enforces a structured approach to pipeline creation.
- **Error Handling**: Built-in error handling features.
- **Extensibility**: Easy to add stages and steps.

### Basic Structure of a Declarative Pipeline

A Declarative Pipeline is defined within a `pipeline` block, which contains stages and steps.

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
```

### Components of a Declarative Pipeline

1. **Pipeline**: The outer block that defines the entire pipeline.
2. **Agent**: Specifies where the pipeline or a specific stage should run.
3. **Stages**: Blocks that define a series of steps.
4. **Steps**: Individual tasks to be performed.

### Detailed Pipeline Syntax

#### 1. Agent

The `agent` directive specifies where the pipeline should run. It can be set at the pipeline level or at the stage level.

```groovy
pipeline {
    agent any // Run on any available agent

    stages {
        stage('Build') {
            agent {
                label 'linux' // Run this stage on an agent with the 'linux' label
            }
            steps {
                echo 'Building...'
            }
        }
    }
}
```

#### 2. Stages

Stages contain a sequence of steps and are used to visualize the progress of the pipeline.

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
```

#### 3. Steps

Steps are individual tasks that are executed within a stage.

```groovy
pipeline {
    agent any

    stages {
        stage('Example') {
            steps {
                echo 'Hello, World!'
            }
        }
    }
}
```

#### 4. Post

The `post` section defines actions that will be taken at the end of the pipeline or a stage.

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }

    post {
        always {
            echo 'This always runs'
        }
        success {
            echo 'This runs on success'
        }
        failure {
            echo 'This runs on failure'
        }
    }
}
```

### Advanced Pipeline Features

#### 1. Environment Variables

You can define environment variables that will be available to all stages.

```groovy
pipeline {
    agent any

    environment {
        APP_NAME = 'my-app'
    }

    stages {
        stage('Build') {
            steps {
                echo "Building ${env.APP_NAME}"
            }
        }
    }
}
```

#### 2. Parameters

Parameters allow you to pass inputs to the pipeline.

```groovy
pipeline {
    agent any

    parameters {
        string(name: 'GREETING', defaultValue: 'Hello', description: 'Greeting message')
    }

    stages {
        stage('Greet') {
            steps {
                echo "${params.GREETING}, World!"
            }
        }
    }
}
```

#### 3. Tools

The `tools` directive allows you to specify tools to be used in the pipeline.

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven 3.6.3'
        jdk 'JDK 11'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn --version'
                sh 'java -version'
            }
        }
    }
}
```

#### 4. Parallel Stages

You can run stages in parallel to speed up the pipeline execution.

```groovy
pipeline {
    agent any

    stages {
        stage('Parallel Stages') {
            parallel {
                stage('Stage 1') {
                    steps {
                        echo 'Running Stage 1'
                    }
                }
                stage('Stage 2') {
                    steps {
                        echo 'Running Stage 2'
                    }
                }
            }
        }
    }
}
```

### Example Scripts Covering Various Features

Here are 20 example scripts covering various methods and tools used in Jenkins pipelines.

#### 1. Simple Build and Test

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
    }
}
```

#### 2. Using Environment Variables

```groovy
pipeline {
    agent any

    environment {
        BUILD_DIR = 'build'
    }

    stages {
        stage('Build') {
            steps {
                echo "Building in directory ${env.BUILD_DIR}"
            }
        }
    }
}
```

#### 3. Using Parameters

```groovy
pipeline {
    agent any

    parameters {
        string(name: 'NAME', defaultValue: 'User', description: 'User name')
    }

    stages {
        stage('Greet') {
            steps {
                echo "Hello, ${params.NAME}!"
            }
        }
    }
}
```

#### 4. Running on a Specific Agent

```groovy
pipeline {
    agent {
        label 'linux'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building on a Linux agent'
            }
        }
    }
}
```

#### 5. Post Actions

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }

    post {
        always {
            echo 'This always runs'
        }
        success {
            echo 'Build was successful'
        }
        failure {
            echo 'Build failed'
        }
    }
}
```

#### 6. Using Tools

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven 3.6.3'
        jdk 'JDK 11'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn --version'
                sh 'java -version'
            }
        }
    }
}
```

#### 7. Parallel Stages

```groovy
pipeline {
    agent any

    stages {
        stage('Parallel Stages') {
            parallel {
                stage('Stage 1') {
                    steps {
                        echo 'Running Stage 1'
                    }
                }
                stage('Stage 2') {
                    steps {
                        echo 'Running Stage 2'
                    }
                }
            }
        }
    }
}
```

#### 8. Conditional Execution

```groovy
pipeline {
    agent any

    stages {
        stage('Conditional Execution') {
            when {
                branch 'main'
            }
            steps {
                echo 'Running on the main branch'
            }
        }
    }
}
```

#### 9. Matrix Builds

```groovy
pipeline {
    agent any

    stages {
        stage('Matrix Build') {
            matrix {
                axes {
                    axis {
                        name 'PLATFORM'
                        values 'linux', 'windows'
                    }
                    axis {
                        name 'VERSION'
                        values '1.0', '2.0'
                    }
                }
                stages {
                    stage('Build') {
                        steps {
                            echo "Building on ${PLATFORM} with version ${VERSION}"
                        }
                    }
                }
            }
        }
    }
}
```

#### 10. Notifications

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }

    post {
        success {
            mail to: 'team@example.com',
                 subject: "Build Successful: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                 body: "Build was successful!"
        }
    }
}
```

#### 11. Checkout from Version Control

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: 'https://github.com/your-repo.git']]])
            }
        }
    }
}
```

#### 12. Running Shell Commands

```groovy
pipeline {
    agent any

    stages {
        stage('Shell Command') {
            steps {
                sh 'echo "Hello from the shell"'
            }
        }
    }
}
```

#### 13. Running on a Docker Agent

```groovy
pipeline {
    agent {
        docker {
            image 'maven:3.6.3-jdk-11'
        }
    }



    stages {
        stage('Build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
```

#### 14. Retry a Stage

```groovy
pipeline {
    agent any

    stages {
        stage('Retry Stage') {
            steps {
                retry(3) {
                    sh 'some-unstable-command'
                }
            }
        }
    }
}
```

#### 15. Using Credentials

```groovy
pipeline {
    agent any

    stages {
        stage('Use Credentials') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-credentials-id', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'echo "User: $USER, Password: $PASS"'
                }
            }
        }
    }
}
```

#### 16. Timestamps in Console Output

```groovy
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }
}
```

#### 17. Archive Artifacts

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'touch artifact.txt'
                archiveArtifacts artifacts: 'artifact.txt', fingerprint: true
            }
        }
    }
}
```

#### 18. Skip Stages Based on Conditions

```groovy
pipeline {
    agent any

    stages {
        stage('Conditional Build') {
            when {
                environment name: 'SKIP_BUILD', value: 'true'
            }
            steps {
                echo 'Skipping build stage because SKIP_BUILD is true'
            }
        }
    }
}
```

#### 19. Use a Shared Library

```groovy
@Library('my-shared-library') _

pipeline {
    agent any

    stages {
        stage('Use Shared Library') {
            steps {
                mySharedLibraryFunction()
            }
        }
    }
}
```

#### 20. Input Step

```groovy
pipeline {
    agent any

    stages {
        stage('Approval') {
            steps {
                script {
                    def userInput = input message: 'Approve deployment?', parameters: [booleanParam(defaultValue: false, description: 'Approve?', name: 'APPROVE')]
                    if (userInput) {
                        echo 'Deployment approved'
                    } else {
                        error 'Deployment not approved'
                    }
                }
            }
        }
    }
}
```

### Conclusion

Jenkins Declarative Pipelines offer a powerful way to define your CI/CD processes with a readable and structured syntax. The examples provided here cover a wide range of features and should serve as a solid foundation for building your pipelines. By leveraging these features, you can create robust, maintainable, and efficient CI/CD pipelines tailored to your project's needs.
