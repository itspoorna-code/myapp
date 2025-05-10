pipeline {
    agent any

    tools {
        maven 'Maven 3.8.4'
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred-id')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/itspoorna-code/myapp.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'  // Use `bat` for Windows
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    // Docker build and push
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-cred-id') {
                        def app = docker.build("itspoornacode/myapp")
                        app.push("latest")
                    }
                }
            }
        }
    }

    post {
        success {
            mail to: 'itspoorna011@gmail.com',
                 subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build Successful!"
        }
        failure {
            mail to: 'itspoorna011@gmail.com',
                 subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build Failed. Check Jenkins for details."
        }
    }
}
