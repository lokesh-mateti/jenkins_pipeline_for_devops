Here are the top 10 most commonly used CI/CD Declarative Pipelines in Jenkins. These pipelines cover the essential stages of software development and deployment processes, ensuring a robust and automated workflow.

### 1. **Basic Build and Test Pipeline**
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/your-project.git'
            }
        }
        stage('Build') {
            steps {
                sh 'make build'
            }
        }
        stage('Test') {
            steps {
                sh 'make test'
            }
        }
    }
}
```

### 2. **Pipeline with Environment Variables**
```groovy
pipeline {
    agent any
    environment {
        APP_ENV = 'production'
    }
    stages {
        stage('Build') {
            steps {
                echo "Building for ${APP_ENV}"
                sh 'make build'
            }
        }
        stage('Test') {
            steps {
                echo "Testing for ${APP_ENV}"
                sh 'make test'
            }
        }
    }
}
```

### 3. **Pipeline with Parallel Stages**
```groovy
pipeline {
    agent any
    stages {
        stage('Build and Test') {
            parallel {
                stage('Build') {
                    steps {
                        sh 'make build'
                    }
                }
                stage('Test') {
                    steps {
                        sh 'make test'
                    }
                }
            }
        }
    }
}
```

### 4. **Pipeline with Docker**
```groovy
pipeline {
    agent {
        docker { image 'maven:3-alpine' }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
```

### 5. **Pipeline with Notifications**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'make build'
            }
        }
        stage('Test') {
            steps {
                sh 'make test'
            }
        }
    }
    post {
        success {
            mail to: 'team@example.com', subject: 'Build Success', body: 'The build was successful.'
        }
        failure {
            mail to: 'team@example.com', subject: 'Build Failed', body: 'The build failed.'
        }
    }
}
```

### 6. **Pipeline with SonarQube Analysis**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
}
```

### 7. **Pipeline with JUnit Test Reporting**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
```

### 8. **Deploy to Kubernetes**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'make build'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                kubernetesDeploy configs: 'k8s-deployment.yml', kubeConfig: [path: 'kubeconfig']
            }
        }
    }
}
```

### 9. **Pipeline with Approval Gate**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'make build'
            }
        }
        stage('Approval') {
            steps {
                input 'Deploy to Production?'
            }
        }
        stage('Deploy') {
            steps {
                sh 'make deploy'
            }
        }
    }
}
```

### 10. **Parameterized Pipeline**
```groovy
pipeline {
    agent any
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/your-repo/your-project.git'
            }
        }
        stage('Build') {
            steps {
                sh 'make build'
            }
        }
        stage('Test') {
            steps {
                sh 'make test'
            }
        }
    }
}
```

These pipelines cover fundamental stages of CI/CD processes, including building, testing, deploying, and integrating with various tools. They provide a solid foundation for automating software development workflows in Jenkins.
