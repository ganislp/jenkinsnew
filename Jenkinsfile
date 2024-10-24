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
        // stage("E2E"){
        //      agent {
        //         docker {
        //            image  'mcr.microsoft.com/playwright:v1.39.0-jammy'
        //            reuseNode true
        //            args '-u root:root'
        //         }
        //     }  
        //     steps{
        //         sh '''
        //             npm install  serve
        //             node_modules/.bin/serve -s build
        //             npx playwright test
        //         '''
        //     }
        // }        
        
    }
    // post {

    //     always {
    //         junit 'test-result/junit.xml'
    //     }
    // }

}
