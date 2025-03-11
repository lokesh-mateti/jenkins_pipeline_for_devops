Here are 50 real-time Jenkins Declarative Pipeline scripts that cover a wide range of use cases and scenarios. Each script is tailored to perform specific tasks that are commonly encountered in CI/CD pipelines.

### Basic Pipelines

1. **Hello World Pipeline**
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Hello') {
         steps {
           echo 'Hello World'
         }
       }
     }
   }
   ```

2. **Simple Build**
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Build') {
         steps {
           sh 'make build'
         }
       }
     }
   }
   ```

3. **Simple Test**
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Test') {
         steps {
           sh 'make test'
         }
       }
     }
   }
   ```

4. **Simple Deploy**
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Deploy') {
         steps {
           sh 'make deploy'
         }
       }
     }
   }
   ```

### Intermediate Pipelines

5. **Pipeline with Environment Variables**
   ```groovy
   pipeline {
     agent any
     environment {
       APP_ENV = 'staging'
     }
     stages {
       stage('Build') {
         steps {
           echo "Building for ${APP_ENV}"
           sh 'make build'
         }
       }
     }
   }
   ```

6. **Pipeline with Notifications**
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Build') {
         steps {
           sh 'make build'
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

7. **Parallel Execution**
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Tests') {
         parallel {
           stage('Unit Tests') {
             steps {
               sh 'make test-unit'
             }
           }
           stage('Integration Tests') {
             steps {
               sh 'make test-integration'
             }
           }
         }
       }
     }
   }
   ```

8. **Conditional Execution**
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Build') {
         when {
           branch 'main'
         }
         steps {
           sh 'make build'
         }
       }
     }
   }
   ```

### Advanced Pipelines

9. **Using Credentials**
   ```groovy
   pipeline {
     agent any
     environment {
       GIT_CREDENTIALS = credentials('git-credentials-id')
     }
     stages {
       stage('Checkout') {
         steps {
           git credentialsId: env.GIT_CREDENTIALS, url: 'https://github.com/example/repo.git'
         }
       }
     }
   }
   ```

10. **Multi-Branch Pipeline**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps {
            echo "Building on branch ${env.BRANCH_NAME}"
            sh 'make build'
          }
        }
      }
    }
    ```

11. **Parameterized Build**
    ```groovy
    pipeline {
      agent any
      parameters {
        string(name: 'TARGET_ENV', defaultValue: 'staging', description: 'Environment to deploy to')
      }
      stages {
        stage('Deploy') {
          steps {
            echo "Deploying to ${params.TARGET_ENV}"
            sh 'make deploy'
          }
        }
      }
    }
    ```

12. **Pipeline with Docker**
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
      }
    }
    ```

### Integration with Other Tools

13. **Pipeline with SonarQube**
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

14. **Pipeline with JUnit**
    ```groovy
    pipeline {
      agent any
      stages {
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

15. **Pipeline with Docker Build and Push**
    ```groovy
    pipeline {
      agent any
      environment {
        DOCKER_CREDENTIALS_ID = 'docker-credentials-id'
      }
      stages {
        stage('Build') {
          steps {
            script {
              docker.build('my-image:latest')
            }
          }
        }
        stage('Push') {
          steps {
            script {
              docker.withRegistry('https://registry.hub.docker.com', env.DOCKER_CREDENTIALS_ID) {
                docker.image('my-image:latest').push('latest')
              }
            }
          }
        }
      }
    }
    ```

### Testing and Quality

16. **Static Code Analysis with Checkstyle**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Checkstyle') {
          steps {
            sh 'mvn checkstyle:checkstyle'
          }
        }
      }
      post {
        always {
          checkstyle pattern: 'target/checkstyle-result.xml'
        }
      }
    }
    ```

17. **Code Coverage with JaCoCo**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Test') {
          steps {
            sh 'mvn test'
          }
        }
        stage('Code Coverage') {
          steps {
            jacoco execPattern: '**/target/*.exec'
          }
        }
      }
    }
    ```

### Deployment Pipelines

18. **Deploy to Kubernetes**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Deploy') {
          steps {
            kubernetesDeploy configs: 'k8s-deployment.yml', kubeConfig: [path: 'kubeconfig']
          }
        }
      }
    }
    ```

19. **Deploy to AWS S3**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Deploy') {
          steps {
            s3Upload bucket: 'my-bucket', path: 'path/to/file', workingDir: 'build'
          }
        }
      }
    }
    ```

### Complex Pipelines

20. **Pipeline with Matrix**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Test') {
          matrix {
            axes {
              axis {
                name 'NODE'
                values 'node1', 'node2'
              }
              axis {
                name 'JDK'
                values '8', '11'
              }
            }
            stages {
              stage('Run Tests') {
                steps {
                  sh "echo Running on ${NODE} with JDK ${JDK}"
                }
              }
            }
          }
        }
      }
    }
    ```

21. **Pipeline with Approval Gate**
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

### Pipelines for Specific Technologies

22. **Node.js Pipeline**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Install Dependencies') {
          steps {
            sh 'npm install'
          }
        }
        stage('Build') {
          steps {
            sh 'npm run build'
          }
        }
        stage('Test') {
          steps {
            sh 'npm test'
          }
        }
      }
    }
    ```

23. **Python Pipeline**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Install Dependencies') {
          steps {
            sh 'pip install -r requirements.txt'
          }
        }
        stage('Lint') {
          steps {
            sh 'flake8 .'
          }
        }
        stage('Test') {
          steps {
            sh 'pytest'
          }
        }
      }
    }
    ```

24. **Java Pipeline**
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
    }
    ```

### Miscellaneous Pipelines

25. **Backup Jenkins Configuration**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Backup') {
          steps {
            sh 'tar -zcvf jenkins_backup.tar.gz $JENKINS_HOME'
          }
        }
      }
    }
    ```

26. **Generate API Documentation**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Generate Docs') {
          steps {
            sh '

apidoc -i src/ -o doc/'
          }
        }
      }
    }
    ```

27. **Run Shell Command**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Run Command') {
          steps {
            sh 'ls -al'
          }
        }
      }
    }
    ```

28. **Archive Artifacts**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
      }
      post {
        success {
          archiveArtifacts artifacts: 'build/*.jar', allowEmptyArchive: true
        }
      }
    }
    ```

### Custom Agents and Advanced Usage

29. **Use Custom Docker Image**
    ```groovy
    pipeline {
      agent {
        docker { image 'custom-image:latest' }
      }
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
      }
    }
    ```

30. **Mount Volume in Docker Agent**
    ```groovy
    pipeline {
      agent {
        docker {
          image 'maven:3-alpine'
          args '-v /my/volume:/mnt/volume'
        }
      }
      stages {
        stage('Build') {
          steps {
            sh 'mvn clean install'
          }
        }
      }
    }
    ```

31. **Using Docker Compose**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Setup') {
          steps {
            sh 'docker-compose up -d'
          }
        }
        stage('Test') {
          steps {
            sh 'docker-compose exec web npm test'
          }
        }
        stage('Teardown') {
          steps {
            sh 'docker-compose down'
          }
        }
      }
    }
    ```

### Pipelines for Specific Scenarios

32. **Pipeline with Retry Logic**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Test') {
          steps {
            retry(3) {
              sh 'make test'
            }
          }
        }
      }
    }
    ```

33. **Pipeline with Timeout**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Long Running Task') {
          steps {
            timeout(time: 1, unit: 'HOURS') {
              sh 'make long-task'
            }
          }
        }
      }
    }
    ```

34. **Build and Tag Git Commit**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
        stage('Tag Commit') {
          steps {
            sh 'git tag -a v1.0.0 -m "Release 1.0.0"'
            sh 'git push origin v1.0.0'
          }
        }
      }
    }
    ```

### Monitoring and Reporting

35. **Send Slack Notification**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
      }
      post {
        success {
          slackSend channel: '#builds', message: 'Build successful!'
        }
        failure {
          slackSend channel: '#builds', message: 'Build failed!'
        }
      }
    }
    ```

36. **Send Build Status to Jira**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
      }
      post {
        success {
          jiraSendBuildInfo site: 'MyJiraSite', buildStatus: 'SUCCESSFUL'
        }
        failure {
          jiraSendBuildInfo site: 'MyJiraSite', buildStatus: 'FAILED'
        }
      }
    }
    ```

### Cleanup and Maintenance

37. **Cleanup Workspace**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
      }
      post {
        always {
          cleanWs()
        }
      }
    }
    ```

38. **Delete Old Builds**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
      }
      post {
        success {
          deleteDir()
        }
      }
    }
    ```

### Security and Compliance

39. **Security Scan with OWASP Dependency Check**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Security Scan') {
          steps {
            sh 'dependency-check --project "My Project" --scan /path/to/project'
          }
        }
      }
      post {
        always {
          dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        }
      }
    }
    ```

40. **SAST with SonarQube**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('SAST') {
          steps {
            withSonarQubeEnv('SonarQubeServer') {
              sh 'mvn sonar:sonar'
            }
          }
        }
      }
    }
    ```

### Advanced Control Flow

41. **Dynamically Generate Stages**
    ```groovy
    def stagesList = ['Build', 'Test', 'Deploy']

    pipeline {
      agent any
      stages {
        stagesList.each { stageName ->
          stage(stageName) {
            steps {
              echo "Running ${stageName} stage"
            }
          }
        }
      }
    }
    ```

42. **Skip Stages Based on User Input**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
        stage('Deploy') {
          when {
            expression { return input(id: 'DeployPrompt', message: 'Deploy to production?', parameters: [booleanParam(defaultValue: true, description: 'Deploy?', name: 'deploy')]) }
          }
          steps {
            sh 'make deploy'
          }
        }
      }
    }
    ```

### Cross-Team Collaboration

43. **Trigger Another Job**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Trigger Job') {
          steps {
            build job: 'other-job', parameters: [string(name: 'param1', value: 'value1')]
          }
        }
      }
    }
    ```

44. **Pipeline with Shared Libraries**
    ```groovy
    @Library('my-shared-library') _
    pipeline {
      agent any
      stages {
        stage('Use Library') {
          steps {
            mySharedLibraryFunction()
          }
        }
      }
    }
    ```

### Multi-Environment Pipelines

45. **Deploy to Multiple Environments**
    ```groovy
    def environments = ['dev', 'staging', 'prod']

    pipeline {
      agent any
      stages {
        environments.each { env ->
          stage("Deploy to ${env}") {
            steps {
              echo "Deploying to ${env}"
              sh "make deploy-${env}"
            }
          }
        }
      }
    }
    ```

### Data Handling and Processing

46. **Process JSON Data**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Process JSON') {
          steps {
            script {
              def json = readJSON file: 'data.json'
              echo "Name: ${json.name}, Age: ${json.age}"
            }
          }
        }
      }
    }
    ```

47. **Generate and Archive Report**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Generate Report') {
          steps {
            sh 'python generate_report.py'
          }
        }
      }
      post {
        success {
          archiveArtifacts artifacts: 'report/*.pdf', allowEmptyArchive: true
        }
      }
    }
    ```

### Miscellaneous Advanced Pipelines

48. **Pipeline with Manual Trigger**
    ```groovy
    pipeline {
      agent any
      triggers {
        cron('H H * * 7')
      }
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
      }
    }
    ```

49. **Send HTTP Request**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Send Request') {
          steps {
            httpRequest url: 'https://api.example.com/endpoint'
          }
        }
      }
    }
    ```

50. **Custom Notifications with Mattermost**
    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps {
            sh 'make build'
          }
        }
      }
      post {
        success {
          mattermostSend message: 'Build successful!', channel: '#builds'
        }
        failure {
          mattermostSend message: 'Build failed!', channel: '#builds'
        }
      }
    }
    ```

These scripts cover a wide range of scenarios and provide a solid foundation for building complex CI/CD pipelines in Jenkins.
