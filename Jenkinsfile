pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                echo 'File List Before Build'
                ls -la

                npm --version
                node --version
                npm ci
                npm run build

                echo 'File List After Build'
                ls -la

                '''
            }
        }
    }
}
