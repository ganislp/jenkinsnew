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
                echo "test started 12"
                npm test 
                '''
            }
        }
        stage ('deploye') {
            agent {
                docker {
                   image  'node:18-alpine'
                   reuseNode true
                }
             steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                '''
            }    
            } 
        }    
    }   
        
    }
    // post {

    //     always {
    //         junit 'test-result/junit.xml'
    //     }
    // }

}
