pipeline {
    agent any

    tools {
        // Ensure Maven is installed on your Jenkins and use the correct version
        maven 'Maven 3.8.4'  // Ensure this Maven version is available in Jenkins Global Tool Configuration
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred-id')  // Set your DockerHub credentials in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub repository
                git branch: 'main', url: 'https://github.com/itspoorna-code/myapp.git'
            }
        }

        stage('Build') {
            steps {
                // Run the Maven build on a Windows agent
                bat 'mvn clean package'  // Use 'bat' for Windows instead of 'sh'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    // Docker build and push
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-cred-id') {
                        def app = docker.build("itspoornacode/myapp")  // Replace with your DockerHub username and app name
                        app.push("latest")
                    }
                }
            }
        }
    }

    post {
        success {
            // Send a success email if build succeeds
            mail to: 'itspoorna011@gmail.com',
                 subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build Successful!"
        }
        failure {
            // Send a failure email if build fails
            mail to: 'itspoorna011@gmail.com',
                 subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build Failed. Check Jenkins for details."
        }
    }
}
