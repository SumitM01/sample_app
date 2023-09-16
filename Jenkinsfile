pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        // AWS_ACCESS_KEY_ID = credentials('AKIATYFGC35IKUF5WGWM')
        // AWS_SECRET_ACCESS_KEY = credentials('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')
        // MICROSOFT_TEAMS_WEBHOOK_URL = credentials('microsoft-teams-webhook-url')
        ECS_CLUSTER_NAME = 'HelloThere'
        ECS_CLUSTER_SERVICE_NAME = 'HelloThere-ecs-svc'
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t hello .'
            }
        }

        stage('Push') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 018772388330.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker tag hello:latest 018772388330.dkr.ecr.us-east-1.amazonaws.com/hello:latest'
                sh 'docker push 018772388330.dkr.ecr.us-east-1.amazonaws.com/hello:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh 'aws ecs update-service --cluster HelloThere --service HelloThere-ecs-svc --force-new-deployment'
            }
        }

        // stage('Notify') {
        //     steps {
        //         httpRequest url: env.MICROSOFT_TEAMS_WEBHOOK_URL, httpMode: 'POST', requestBody: JsonOutput.toJson([text: "Deployment completed!"])
        //     }
        // }
    }
}
