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
### EXAMPLE 4 -
```
pipeline {
   agent any
   environment {
       arghya = 'suvo'                               //can be used in whole pipeline
   }
   stages {
       stage(“Build”) {
           steps {
           withEnv(["arghya=suvo"]) {
                    echo "arghya = ${env.arghya}"
                }
       }
   }
}
}
```
### CHANGING VARIABLE VALUE -
```
pipeline {
   agent any
   environment {
       arghya = 'suvo'                               //can be used in whole pipeline
   }
   stages {
       stage(“Build”) {
           steps {
           script {
               env.arghya = 'name'
               echo 'hello ' + env.arghya + ' roy'
           }
           }
       }
   }
}
```
### ALL WAYS TO USE VARIABLE -
```
pipeline {
    agent any

    environment {
        FOO = "bar"
        NAME = "Joe"
    }

    stages {
        stage("Env Variables") {
            environment {
                NAME = "Alan" // overrides pipeline level NAME env variable
                BUILD_NUMBER = "2" // overrides the default BUILD_NUMBER
            }

            steps {
                echo "FOO = ${env.FOO}" // prints "FOO = bar"
                echo "NAME = ${env.NAME}" // prints "NAME = Alan"
                echo "BUILD_NUMBER =  ${env.BUILD_NUMBER}" // prints "BUILD_NUMBER = 2"

                script {
                    env.SOMETHING = "1" // creates env.SOMETHING variable
                }
            }
        }

        stage("Override Variables") {
            steps {
                script {
                    env.FOO = "IT DOES NOT WORK!" // it can't override env.FOO declared at the pipeline (or stage) level
                    env.SOMETHING = "2" // it can override env variable created imperatively
                }

                echo "FOO = ${env.FOO}" // prints "FOO = bar"
                echo "SOMETHING = ${env.SOMETHING}" // prints "SOMETHING = 2"

                withEnv(["FOO=foobar"]) { // it can override any env variable
                    echo "FOO = ${env.FOO}" // prints "FOO = foobar"
                }

                withEnv(["BUILD_NUMBER=1"]) {
                    echo "BUILD_NUMBER = ${env.BUILD_NUMBER}" // prints "BUILD_NUMBER = 1"
                }
            }
        }
    }
}
```
### Storing Boolean values in environment variables
```
pipeline {
    agent any

    environment {
        IS_BOOLEAN = false
    }

    stages {
        stage("Env Variables") {
            steps {
                script {
                    if (env.IS_BOOLEAN) {
                        echo "You can see this message, because \"false\" String evaluates to Boolean.TRUE value"
                    }

                    if (env.IS_BOOLEAN.toBoolean() == false) {
                        echo "You can see this message, because \"false\".toBoolean() returns Boolean.FALSE value"
                    }
                }
            }
        }
    }
}
```
### Capturing sh command output in the env variable -
You can also capture output of a shell command as an environment variable. Keep in mind that you need to use sh(script: 'cmd', returnStdout:true) format to force sh step[2] to return an output so it can be captured and stored in a variable. Also, if you want to avoid capturing the new line character that is usually present in the shell command output, call trim() method to remove all whitespace from the beginning and the end of the captured output.
```
pipeline {
    agent any

    environment {
        LS = "${sh(script:'ls -lah', returnStdout: true).trim()}"
    }

    stages {
        stage("Env Variables") {
            steps {
                echo "LS = ${env.LS}"
            }
        }
    }
}
```
## SCRIPT INSIDE STEP -
```
pipeline {
    agent any

    environment {
        FOO = "bar"
    }

    stages {
        stage("Env Variables") {
            environment {
                NAME = "Alan"
            }

            steps {
                echo "FOO = ${env.FOO}"
                echo "NAME = ${env.NAME}"

                script {
                    env.TEST_VARIABLE = "some test value"
                }

                echo "TEST_VARIABLE = ${env.TEST_VARIABLE}"

                withEnv(["ANOTHER_ENV_VAR=here is some value"]) {
                    echo "ANOTHER_ENV_VAR = ${env.ANOTHER_ENV_VAR}"
                }
            }
        }
    }
}
```
## PARALLEL JOBS -
```
pipeline {
    agent any
    stages {
        stage ('parallel works')
        {
            parallel {
        stage('test') {
            steps {
                sh 'echo test'
            }
        }
        stage('test1') {
            steps {
                sh 'echo test1'
            }
        }
        stage('test2') {
            steps {
                sh 'echo test2'
            }
        }
    }
  }
}
}
```
