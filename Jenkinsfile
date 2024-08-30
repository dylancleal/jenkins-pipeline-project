pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing the code...'
                sh 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                sh 'dependency-check'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                sh 'aws deploy --application-name MyApp --deployment-group staging-group --s3-location s3://my-bucket/my-app.zip'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                sh 'mvn verify'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                sh 'aws deploy --application-name MyApp --deployment-group production-group --s3-location s3://my-bucket/my-app.zip'
            }
        }
    }

    post {
        success {
            mail to: 'developer@example.com',
                 subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "The pipeline succeeded. Check console output at ${env.BUILD_URL} to view the results."
        }
        failure {
            mail to: 'developer@example.com',
                 subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "The pipeline failed. Check console output at ${env.BUILD_URL} to view the results.",
                 attachLog: true
        }
    }
}
