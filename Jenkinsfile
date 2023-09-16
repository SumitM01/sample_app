pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-west-2'
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        MICROSOFT_TEAMS_WEBHOOK_URL = credentials('microsoft-teams-webhook-url')
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t hello-world-app .'
            }
        }

        stage('Push') {
            steps {
                sh 'docker tag hello-world-app:latest <your-docker-repo>/hello-world-app:latest'
                sh 'docker push <your-docker-repo>/hello-world-app:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh 'aws ecs update-service --cluster <your-cluster-name> --service <your-service-name> --force-new-deployment'
            }
        }

        stage('Notify') {
            steps {
                httpRequest url: env.MICROSOFT_TEAMS_WEBHOOK_URL, httpMode: 'POST', requestBody: JsonOutput.toJson([text: "Deployment completed!"])
            }
        }
    }
}
