## IF - ELSE STATEMENT -
### Example 1 -
```
pipeline {
    agent any

    stages {
        stage('test') {
            steps {
                sh 'echo hello'
            }
        }
        stage('test1') {
            steps {
                sh 'echo $TEST'
            }
        }
        stage('test3') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') 
                        {
                        echo 'Hello from main branch'
                        }
                    if (env.BRANCH_NAME == 'null') 
                        {
                        echo 'Hello from null branch'
                        }
                    else {
                        sh "echo 'Hello from ${env.BRANCH_NAME} branch!'"
                        }
                }
            }
        }
    }
}
```
## SET ENVIRONMENT VARIABLES -
### EXAMPLE 1 - 
```
pipeline {
    agent any
environment {
       arghya = 'suvo'                               //can be used in whole pipeline
   }
    stages {
        stage('test') {
            steps {
                sh 'echo hello'
            }
        }
        stage('test1') {
            steps {
                sh 'echo $TEST'
            }
        }
        stage('test3') {
            steps {
                script {
                    if (env.arghya == 'suvo') 
                        {
                        echo 'Hello from suvo'
                        }
                    else {
                        sh "echo 'Hello from non suvo!'"
                        }
                }
            }
        }
    }
}
```
### EXAMPLE 2 -
```
pipeline {
   agent any
   environment {
       arghya = 'suvo'                               //can be used in whole pipeline
   }
   stages {
       stage(“Build”) {
           steps {
               echo 'hello ' + env.arghya + ' roy'
           }
       }
   }
}
```
### EXAMPLE 3 -
```
pipeline {
   agent any
   environment {
       name = 'arghya'
   }
   stages {
       stage(“Build”) {
           environment {
                   surname = ‘roy’                                 //can be used in this stage only
              }
           steps {
               echo env.name
               echo env.surname
           }
       }
   }
}
```
