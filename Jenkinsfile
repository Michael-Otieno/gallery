pipeline {
    agent any
    tools {nodejs "NodeJS"}

    environment {
        RENDER_SERVICE_ID = credentials('render-id') 
        RENDER_API_KEY = credentials('api-key') 
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

        stage('Test code'){
            steps{
                sh 'npm test'
            }
        }
        
        stage('Deployment'){
            steps{
                echo 'Deploying to render'
                script {
                    sh """
                    curl -X POST \
                        https://api.render.com/deploy/${RENDER_SERVICE_ID}?key=${RENDER_API_KEY}
                    """
                }
            }
        }
    }
}
