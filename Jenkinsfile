pipeline {
    agent any
    tools {nodejs "NodeJS"}

    environment {
        RENDER_SERVICE_ID = credentials('render-id') 
        RENDER_API_KEY = credentials('api-key') 
        NOTIFICATION_EMAIL = credentials('notification-email')
        SITE_URL = credentials('site-url')
    }

    stages {
        stage('clone repository') {
            steps {
                git branch:'master',url:'https://github.com/Michael-Otieno/gallery'
            }
        }
        stage('Dependency installation'){
            steps{
                sh 'npm install'
            }
        }

        stage('Test'){
            steps{
                sh 'npm test'
            }
        }
        
        stage('Deployment'){
            steps{
                script {
                    sh 'curl -X POST https://api.render.com/deploy/${RENDER_SERVICE_ID}?key=${RENDER_API_KEY}'
                }
            }
        }
    }

     post {
        always {
            echo 'Cleaning up resources...'
        }

         success {
            echo 'Build and tests succeeded. Sending success notification...'
            // emailext(
            //     subject: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            //     body: """
            //         <html>
            //             <body style="color:'#69db7c'">
            //                 <p>The build <b>#${env.BUILD_NUMBER}</b> of job <b>${env.JOB_NAME}</b> was successful.</p>
            //                 <p>Check the details at: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
            //             </body>
            //         </html>
            //     """,
            //     mimeType: 'text/html',
            //     to: "${env.NOTIFICATION_EMAIL}"
            // )
            slackSend(
                channel: '#michael_ip1', 
                color: 'good', 
                message: "Build Succeeded: #${env.BUILD_NUMBER} ${env.SITE_URL}")

        }
        
        failure {
            echo 'Tests failed. Sending notification...'
            emailext(
                subject: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <html>
                        <body>
                            <p>The build <b>#${env.BUILD_NUMBER}</b> of job <b>${env.JOB_NAME}</b> was successful.</p>
                            <p>Check the details at: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                        </body>
                    </html>
                """,
                mimeType: 'text/html',
                to: "${env.NOTIFICATION_EMAIL}"
            )

        }

        
    }


}
