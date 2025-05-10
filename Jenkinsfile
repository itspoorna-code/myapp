pipeline {
    agent any

    tools {
        maven 'Maven 3.8.4'  // Ensure Maven is configured in Jenkins > Global Tool Configuration
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred-id') // Set this in Jenkins Credentials
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/itspoorna-code/myapp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        def app = docker.build('itspoorna/myapp') // Replace with your actual DockerHub username if different
                        app.push('latest')
                    }
                }
            }
        }
    }

    post {
        success {
            mail to: 'itspoorna011@gmail.com',
                 subject: "✅ Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "The Jenkins build was successful.\n\nCheck it here: ${env.BUILD_URL}"
        }

        failure {
            mail to: 'itspoorna011@gmail.com',
                 subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "The Jenkins build failed.\n\nCheck it here: ${env.BUILD_URL}"
        }
    }
}
