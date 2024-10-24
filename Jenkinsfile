pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = '2f4d56e6-880b-463d-bcaf-26bb6bbd0075'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        REACT_APP_VERSION = "1.0.$BUILD_ID"

       
    }
    stages {
        stage('AWS'){
            agent {
                docker {
                    image 'amazon/aws-cli'
                    args "--entrypoint=''"
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                 sh '''
                      aws --version
                      aws s3 ls
                      echo "Hello s3!" > index.html
                      aws s3 cp index.html s3://jenkines-test-123444444/index.html
                 '''
                }                

            }
        }
        stage('Build') {
            agent {
                docker {
                   image  'my-paywrite'
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
                   image  'my-paywrite'
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
                   image  'my-paywrite'
                   reuseNode true
                }
            }  
            steps{
                sh '''
                    netlify --version
                    echo "Deploying to staging. Site ID : $NETLIFY_SITE_ID"
                    netlify deploy --dir=build --json > deploy-output.json
                    
                '''
            script {
                env.STAGING_URL = sh(script:"node-jq -r '.deploy_url' deploy-output.json",returnStdout: true)
            }   

            }

        }
        stage("Deploy prod"){
             agent {
                docker {
                   image  'my-paywrite'
                   reuseNode true
                }
            }  
            steps{
                sh '''
                    netlify --version
                    echo "Deploying to production. Site ID : $NETLIFY_SITE_ID"
                    netlify deploy --dir=build --prod
                '''
            }
        }  
        stage("Stage E2E"){ 
            steps {
               echo "STAGE_URL is -  ${env.STAGING_URL}"
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
