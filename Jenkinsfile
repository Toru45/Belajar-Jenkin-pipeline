pipeline {
    agent {
        node {
            label "linux"
        }
    }
    stages {
        stage('Hello') {
            steps {
                echo("Hello pipeline!")
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