pipeline {
    agent any
    tools {nodejs "NodeJS"}

    stages {
        stage('clone repository') {
            steps {
                echo 'cloning repository'
                git branch:'master',url:'https://github.com/Michael-Otieno/gallery'
            }
        }
        stage('install npm'){
            steps{
                echo 'installing npm'
                sh 'npm install'
            }
        }
    }
}
