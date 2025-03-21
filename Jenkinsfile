pipeline {
    agent {
        node {
            label "linux"
        }
    }
    environment {
        AUTHOR = 'Faiq'
    }



    parameters{
        string(name: "NAME", defaultValue: "Faiq", description: "What is your name")
        text(name: "DESCRIPTION", defaultValue: "Faiq", description: "Tell me about yourself")
        booleanParam(name: "DEPLOY", defaultValue: false, description: "Need to deploy?")
        choice(name: "SOCIAL_MEDIA",choices: ['Instagram','Facebook', 'Twitter'], description: "Which social media do you use?")
        password(name: "SECRET", defaultValue: "", description: "Encrypt key")
    }

    options {
        disableConcurrentBuilds()
    }
    
    tools {
        jdk "JDK17"
    }
    stages {
        stage("OS Setup") {
            matrix {
                axes {
                    axis {
                        name 'OS'
                        values 'Linux', 'Windows', 'mac'
                    }
                    axis {
                        name "ARC"
                        values "32", "64"
                    }
                }
                excludes{
                    exclude{
                        axis {
                        name "OS"
                        values "mac"
                    }
                        axis {
                            name "ARC"
                            values "32"
                        }
                    }  
                }
                stages {
                    stage("OS Setup") {
                        steps {
                            echo("Setup ${OS} ${ARC}")
                        }
                    }
                }
            }
        }

        stage('Preparation') {
            parallel {
                stage("Prepare Java") {
                    steps {
                        echo("Preparing Java...")
                        sh('java -version')
                        sleep(5)
                    }
                }
                stage('Prepare Maven') {
                    steps {
                        echo("Preparing Maven...")
                        sleep(5)
                    }
                }
            }       
        }
        stage('Hello') {
            steps {
                echo("Hello pipeline!")
            }
        }

        stage('Parameter'){
            steps {
                echo("Name : ${params.NAME}")
                echo("Description : ${params.DESCRIPTION}")
                echo("Deploy : ${params.DEPLOY}")
                echo("Social Media : ${params.SOCIAL_MEDIA}")
                echo("Secret : ${params.SECRET}")   
            }
        }

        stage('prepare') {
            environment {
                APP = credentials("faiq_rahasia")
            }
            steps {
                echo("Author : ${AUTHOR}")
                echo("Start Job : ${env.JOB_NAME}")
                echo("Start Build : ${env.BUILD_NUMBER}")
                echo("Branch Name : ${env.BRANCH_NAME}")
                echo("App User : ${APP_USR}")
                echo("APP password : ${APP_PSW}")
                sh('echo "App Password : $APP_PSW" > "rahasia.txt"')
            }
        }
        stage('Build') {
            steps {
                echo("Start Build...")
                sh ("./mvnw clean compile test-compile")
                echo("Finish Build...")
            }
        }
        stage('Test') {
            steps {
                script {
                    def data = [
                        "firstname" : "Faiq",
                        "Lastname" : "Radi"
                    ]
                    writeJSON(file: 'data.json', json: data)
                }
                echo("Start Test...")
                sh ("./mvnw test")
                echo("Finish test...")
            }
        }
        stage('Deploy') {
            input {
                    message "can we deploy"
                    ok "Yes"
                    submitter "Faiqradi"
                }
            steps {
                echo("Deploying pipeline...")
            }
        }

        stage('release') {
            when {
                expression {
                    return params.DEPLOY
                }
            }
            steps {
                withCredentials([usernamePassword(
                    credentialsId : "faiq_rahasia",
                    usernameVariable : "USER",
                    passwordVariable : "PASSWORD"
                )]){
                    sh('echo "Release it with -u $USER -p $PASSWORD" > "release.txt"')
                }
            }
        }
    }

    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        cleanup {
            echo 'This will always run'
        }
    }
}