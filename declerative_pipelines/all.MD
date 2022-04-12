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
                   else if (env.BRANCH_NAME == 'null') 
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
### Variables without env.
```
pipeline {
    agent any
    environment {
       roy = 'suvo'                               //can be used in whole pipeline
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
                    if (env.roy == 'main') 
                        {
                        echo 'Hello from main branch'
                        }
                   else if (env.roy == 'suvo') 
                        {
                        echo "Hello from $roy branch"
                        echo "Hello from $BUILD_NUMBER branch"
                        }
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
##  TRY - CATCH BLOCK - 
This is a process of error handling. It helps us to execute other code after one code fails. Suppose content of try block fails then the error will
be catched by catch block, and after that catch block executes another codes. We can also use finally block (optional) with try catch. Finally block
executes independent of try or catch. Finally block must be executed either try block succesful or not.
We can use 
- try-catch
- try-catch-finally
- try-finally
```
pipeline {
    agent any

    stages {
        stage('Hello2') {
           
            steps {
                 script {
                        try {
                        sh 'docker ps'
                             } 
                        catch (Exception exp) {
                        sh 'echo "Handle the exception!"'
                                               }
                        finally {
                            sh 'echo "try-catch block sucessful"'
                        }
                        
                          }
                  echo 'Hello World'
            }
        }
    }
}
```
## PARAMETER -
```
pipeline {
    agent any
    stages {
            stage('Setup parameters1') {
            steps {
                script { 
properties([parameters([booleanParam(defaultValue: true, description: 'description', name: 'boolean1'), choice(choices: ['one', 'two', 'three'], description: 'description', name: 'choice1')])])                }
            }
            }
    stage('Deploy to Production') {
            when {
                expression { 
                   return params.choice1 == 'two'
                }
            }
            steps {
                    sh """
                    echo "deploy to production"
                    """
                }
            }
            
            
   }
            }

```
## Using Active Choice Parameter in Declarative Pipeline for Dynamic Parameters -
Unlike default parameter types, the Active choice parameter type gives you more control over the parameters using a groovy script. You can have dynamic parameters based on user parameter selection.

To use the active choice parameter, you need to have an Active Choices plugin installed in Jenkins.

Here is a small use case for an active choice parameter.

A job should have three parameters
- Environment (dev, stage & prod)
- AMI List (Should list the AMIs based on environment)
- AMI information (Show information about the AMIs related to a specific environment)
If the user selects dev, the AMI list should dynamically change the values related to dev and show information related to the AMIs.
```
pipeline {
    agent any
        stages {
            stage('Parameters'){
                steps {
                    script {
                    properties([
                            parameters([
                                [$class: 'ChoiceParameter', 
                                    choiceType: 'PT_SINGLE_SELECT', 
                                    description: 'Select the Environemnt from the Dropdown List', 
                                    filterLength: 1, 
                                    filterable: false, 
                                    name: 'Env', 
                                    script: [
                                        $class: 'GroovyScript', 
                                        fallbackScript: [
                                            classpath: [], 
                                            sandbox: false, 
                                            script: 
                                                "return['Could not get The environemnts']"
                                        ], 
                                        script: [
                                            classpath: [], 
                                            sandbox: false, 
                                            script: 
                                                "return['dev','stage','prod']"
                                        ]
                                    ]
                                ],
                                [$class: 'CascadeChoiceParameter', 
                                    choiceType: 'PT_SINGLE_SELECT', 
                                    description: 'Select the AMI from the Dropdown List',
                                    name: 'AMI List', 
                                    referencedParameters: 'Env', 
                                    script: 
                                        [$class: 'GroovyScript', 
                                        fallbackScript: [
                                                classpath: [], 
                                                sandbox: false, 
                                                script: "return['Could not get Environment from Env Param']"
                                                ], 
                                        script: [
                                                classpath: [], 
                                                sandbox: false, 
                                                script: '''
                                                if (Env.equals("dev")){
                                                    return["ami-sd2345sd", "ami-asdf245sdf", "ami-asdf3245sd"]
                                                }
                                                else if(Env.equals("stage")){
                                                    return["ami-sd34sdf", "ami-sdf345sdc", "ami-sdf34sdf"]
                                                }
                                                else if(Env.equals("prod")){
                                                    return["ami-sdf34sdf", "ami-sdf34ds", "ami-sdf3sf3"]
                                                }
                                                '''
                                            ] 
                                    ]
                                ],
                                [$class: 'DynamicReferenceParameter', 
                                    choiceType: 'ET_ORDERED_LIST', 
                                    description: 'Select the  AMI based on the following infomration', 
                                    name: 'Image Information', 
                                    referencedParameters: 'Env', 
                                    script: 
                                        [$class: 'GroovyScript', 
                                        script: 'return["Could not get AMi Information"]', 
                                        script: [
                                            script: '''
                                                    if (Env.equals("dev")){
                                                        return["ami-sd2345sd:  AMI with Java", "ami-asdf245sdf: AMI with Python", "ami-asdf3245sd: AMI with Groovy"]
                                                    }
                                                    else if(Env.equals("stage")){
                                                        return["ami-sd34sdf:  AMI with Java", "ami-sdf345sdc: AMI with Python", "ami-sdf34sdf: AMI with Groovy"]
                                                    }
                                                    else if(Env.equals("prod")){
                                                        return["ami-sdf34sdf:  AMI with Java", "ami-sdf34ds: AMI with Python", "ami-sdf3sf3: AMI with Groovy"]
                                                    }
                                                    '''
                                                ]
                                        ]
                                ]
                            ])
                        ])
                    }
                }
            }
        }   
}
```
https://devopscube.com/declarative-pipeline-parameters/
## CONDITION WITH ENVIRONMENT VARIABLE - 
```
pipeline {
    agent any
    environment {
        ENV = "testing"
        ENV1 = "arghya"
    }
    stages {
        stage("Test") {
            when {
                environment(name: "ENV", value: "testing")
            }
            steps {
                echo "Test stage."
            }
        }
        stage("Test2") {
            when {
                environment(name: "ENV1", value: "arghya")
            }
            steps {
                echo "Test stage."
            }
        }
    }
}
```
## MULTIBRANCH PIPELINE -
```
pipeline {
    agent any
    stages {
        stage('Master Branch Deploy Code') {
            when {
                branch 'production'
            }
            steps {
                sh """
                echo "Building Artifact from Master branch"
                """
 
                sh """
                echo "Deploying Code from Master branch"
                """
            }
        }
        stage('Develop Branch Deploy Code') {
            when {
                branch 'development'
            }
            steps {
                sh """
                echo "Building Artifact from Develop branch"
                """
                sh """
                echo "Deploying Code from Develop branch"
                """
           }
        }
    }
}
```
## BUILD PIPELINE IN SELECTIVE SLAVE -
```
pipeline {
agent {label 'node1'}

stages {
    stage('Hello') {
        steps {
            echo 'Hello World'
        }
    }
}
}
```
