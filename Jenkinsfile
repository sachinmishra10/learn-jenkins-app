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
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
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
        stage('E2E'){
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps{
                sh '''
                echo 'E2E Tetsing....'
                npm install  serve
                node_modules/.bin/serve -s build &
                sleep 10
                npx playwright test  --reporter=line
                '''
            }
        }
    }
    post{
        always{
            junit 'jest-results/junit.xml'
        }
    }
}
