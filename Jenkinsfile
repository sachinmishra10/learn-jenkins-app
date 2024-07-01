pipeline {
    agent any
    environment{
        Test_File_Name= 'build/index.html'
        Test_File_JUnit_Summery='test-results/junit.xml'
    }
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
        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                echo 'Testing....'
                test -f $Test_File_Name
                npm  test a

                '''
            }
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}
