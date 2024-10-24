pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = '2f4d56e6-880b-463d-bcaf-26bb6bbd0075'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
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
                echo "test started 12366"
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
                    echo "Deploying to production. Site ID : $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''

            }
        }                    
    }
}
