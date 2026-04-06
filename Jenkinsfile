pipeline {
    agent { label 'agent2' }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timeout(time: 15, unit: 'MINUTES')
    }

    stages {
        stage('Kubernetes Deploy') {
            steps {
                script {
                    sh 'kubectl cluster-info'

                    sh 'kubectl apply -f k8s/'

                    sh 'kubectl rollout restart deployment/job4j-devops'

                    sh 'kubectl rollout status deployment/job4j-devops'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
            script {
                def status = currentBuild.currentResult
                def emoji = status == 'SUCCESS' ? '✅' : status == 'FAILURE' ? '❌' : '⚠️'
                def duration = currentBuild.durationString.replace(' and counting', '')
                def buildInfo = """
${emoji} *${env.JOB_NAME}* — #${currentBuild.number}
━━━━━━━━━━━━━━━━━━━━
📌 Status: *${status}*
🕐 Started: ${new Date(currentBuild.startTimeInMillis).format('dd.MM.yyyy HH:mm:ss')}
⏱ Duration: ${duration}
━━━━━━━━━━━━━━━━━━━━
🔗 [Open in Jenkins](${env.BUILD_URL})
                """.trim()
                telegramSend(message: buildInfo)
            }
        }
        success {
            echo "Successfully deployed to Kubernetes!"
        }
        failure {
            echo "Deployment failed! Check kubectl logs."
        }
    }
}