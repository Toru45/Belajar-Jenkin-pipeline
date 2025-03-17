pipeline {
    agent {
        node {
            label "linux"
        }
    }
    
    tools {
        jdk "JDK17"
    }
    stages {
        stage('Hello') {
            steps {
                echo("Hello pipeline!")
            }
        }

        stage('prepare') {
            steps {
                echo("Start Job : ${env.JOB_NAME}")
                echo("Start Build : ${env.BUILD_NUMBER}")
                echo("SBranch Name : ${env.BRANCH_NAME}")
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
            steps {
                echo("Deploying pipeline...")
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