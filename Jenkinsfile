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
                echo 'cloning repository'
                git branch:'master',url:'https://github.com/Michael-Otieno/gallery'
            }
        }
        stage('Dependency installation'){
            steps{
                echo 'installing npm'
                sh 'npm install'
            }
        }
         stage('Setup MongoDB') {
            steps {
                echo 'Setting up MongoDB'
                script {
                    sh '''
                    # Update system packages
                    sudo apt-get update

                    # Install MongoDB
                    sudo apt-get install -y mongodb

                    # Start MongoDB service
                    sudo service mongod start

                    # Verify MongoDB is running
                    mongo --eval "db.runCommand({ connectionStatus: 1 })"
                    '''
                }
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
