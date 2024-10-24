pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                   image  'node:18-alpine'
                   reuseNode true
                }
            }  
            steps {
             sh '''
               ls -la
               node --version
               npm ci
               npm run build
               ls -la
             '''
            }
        }
        stage("Test"){
             agent {
                docker {
                   image  'node:18-alpine'
                   reuseNode true
                }
            }  
            steps{
                sh '''
                echo "test started 123"
                npm test 
                '''
            }
        }  
        stage("Deploy"){
             agent {
                docker {
                   image  'node:18-alpine'
                   reuseNode true
                }
            }  
            steps{
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                '''

            }
        }                    
    }
}
