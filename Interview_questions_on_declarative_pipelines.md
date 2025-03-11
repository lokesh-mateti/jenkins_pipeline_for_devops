
### Basic Questions
1. **What is Jenkins Declarative Pipeline?**
   - Answer: Jenkins Declarative Pipeline is a simplified syntax for defining continuous integration and continuous delivery (CI/CD) pipelines in Jenkins. It uses a structured, easy-to-read format that allows users to describe their pipeline process in a straightforward manner.

2. **What is the difference between Declarative Pipeline and Scripted Pipeline?**
   - Answer: Declarative Pipeline provides a more simplified and structured approach to defining pipelines, using predefined blocks and syntax. Scripted Pipeline is more flexible and allows for complex logic using Groovy scripting but can be harder to read and maintain.

3. **Can you describe the basic structure of a Declarative Pipeline?**
   - Answer: A basic Declarative Pipeline starts with a `pipeline` block, which contains `agent`, `stages`, and `post` blocks. Each `stage` block contains `steps` that define the actions to be performed.

### Intermediate Questions
4. **How do you specify the agent for a Declarative Pipeline?**
   - Answer: The agent can be specified at the top level of the pipeline to define where the entire pipeline will run, or within individual stages to define stage-specific agents. Syntax examples:
     ```groovy
     pipeline {
       agent any
       stages {
         stage('Build') {
           agent { label 'docker' }
           steps {
             // steps go here
           }
         }
       }
     }
     ```

5. **What are some common `post` actions in a Declarative Pipeline?**
   - Answer: Common `post` actions include `always`, `success`, `failure`, and `unstable`. These actions define steps that should be executed after the pipeline or stage completes based on the pipeline's status.

6. **How can you use environment variables in a Declarative Pipeline?**
   - Answer: Environment variables can be defined globally within the `environment` block or locally within a `stage`. Example:
     ```groovy
     pipeline {
       environment {
         MY_VAR = 'value'
       }
       stages {
         stage('Example') {
           steps {
             echo "Value of MY_VAR is ${env.MY_VAR}"
           }
         }
       }
     }
     ```

### Advanced Questions
7. **How do you handle parallel execution in Declarative Pipelines?**
   - Answer: Parallel execution can be defined using the `parallel` directive within a stage. Example:
     ```groovy
     pipeline {
       agent any
       stages {
         stage('Parallel Stage') {
           parallel {
             stage('Unit Tests') {
               steps {
                 // steps for unit tests
               }
             }
             stage('Integration Tests') {
               steps {
                 // steps for integration tests
               }
             }
           }
         }
       }
     }
     ```

8. **How can you use `when` conditions to control the execution flow in a Declarative Pipeline?**
   - Answer: The `when` block allows for conditional execution of stages or steps based on certain criteria. Example:
     ```groovy
     pipeline {
       agent any
       stages {
         stage('Build') {
           when {
             branch 'main'
           }
           steps {
             // build steps
           }
         }
       }
     }
     ```

9. **Explain how to use and share libraries in Jenkins Declarative Pipeline.**
   - Answer: Shared libraries can be used to encapsulate reusable code. They are typically defined in a `vars` directory within a shared repository and then loaded in the pipeline. Example:
     ```groovy
     @Library('my-shared-library') _
     pipeline {
       agent any
       stages {
         stage('Example') {
           steps {
             mySharedLibraryFunction()
           }
         }
       }
     }
     ```

10. **How do you handle credentials securely in a Declarative Pipeline?**
    - Answer: Credentials can be stored in Jenkins' credentials store and accessed using the `credentials` directive. Example:
      ```groovy
      pipeline {
        agent any
        environment {
          MY_SECRET = credentials('my-secret-id')
        }
        stages {
          stage('Use Secret') {
            steps {
              echo "My secret is ${MY_SECRET}"
            }
          }
        }
      }
      ```

### Scenario-Based Questions
11. **Describe how you would set up a multi-branch pipeline using Declarative Pipeline syntax.**
    - Answer: A multi-branch pipeline automatically detects branches and pull requests in the repository. You set up the Jenkins job as a multi-branch pipeline, and Jenkins will use the `Jenkinsfile` in each branch. Example `Jenkinsfile`:
      ```groovy
      pipeline {
        agent any
        stages {
          stage('Build') {
            steps {
              echo "Building on branch ${env.BRANCH_NAME}"
            }
          }
        }
      }
      ```

12. **How do you deal with long-running pipelines and timeouts in Declarative Pipelines?**
    - Answer: You can set timeouts for stages or entire pipelines using the `options` block. Example:
      ```groovy
      pipeline {
        agent any
        options {
          timeout(time: 1, unit: 'HOURS')
        }
        stages {
          stage('Long Running Task') {
            steps {
              script {
                sleep 3600 // simulate long task
              }
            }
          }
        }
      }
      ```

