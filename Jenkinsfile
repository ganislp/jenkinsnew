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
                echo "test started 12366666"
                npm test 
                '''
            }
        }  
        stage("Deploy staging"){
             agent {
                docker {
                   image  'node:18-alpine'
                   reuseNode true
                }
            }  
            steps{
                sh '''
                     npm install netlify-cli node-jq
                    node_modules/.bin/netlify --version
                    echo "Deploying to staging. Site ID : $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify deploy --dir=build --json > deploy-output.json
                    node_modules/.bin/node-jq -r '.deploy_url' deploy-output.json
                '''
            }
        }
        stage("Deploy prod"){
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
        stage("Stage E2E"){ 
            steps {
               echo "STAGE_URL is "
            }

        }     

    //     stage("Prod E2E"){
    //         environment {
    //            CI_ENVIRONMENT_URL = 'https://resonant-granita-07b668.netlify.app' 
    //         }
    //         agent {
    //             docker {
    //                image  'mcr.microsoft.com/playwright:v1.39.0-jammy'
    //                reuseNode true
    //             }
	// 			}   
    //         steps {
    //             sh '''
    //                  npx playwright test  --reporter=html
    //                  '''
    //                 }
    //         post {
    //             always {
    //                  publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright E2E', reportTitles: '', useWrapperFileDirectly: true])
    //                     }
    //             }                                        
    // }
    }
}
