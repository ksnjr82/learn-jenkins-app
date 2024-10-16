pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = '7b1855f2-3597-4edd-aa7d-8608fff00b12'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
    stages {
        stage('Build') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
                
            }
        }
        stage ('Test') {
          agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
          }
            steps { 
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }
stage('Deploy') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                
                sh '''
                  npm install netlify-cli 
                  ./node_modules/.bin/netlify --version
                  echo " Deploying to production. Site ID: $NETLIFY_SITE_ID"
                  ./node_modules/.bin/netlify status

                '''
                
            }
        }

    }

        post {always {
            junit 'test-results/junit.xml'
        }}
}
