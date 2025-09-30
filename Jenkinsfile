pipeline {
    agent any
    environment {
        // Define any environment variables here
        NETLIFY_SITE_ID = '00d484b7-a9e3-43f8-b78d-064fe12116ec'
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    // args '-v /var/run/docker.sock:/var/run/docker.sock'
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
                // Add your build steps here
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:20-alpine'
                    // args '-v /var/run/docker.sock:/var/run/docker.sock'
                    reuseNode true
                }
            }
            steps {
                sh '''
                test -f build/index.html
                npm test
                '''
                // Add your test steps here
            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:20-alpine'
                    // args '-v /var/run/docker.sock:/var/run/docker.sock'
                    reuseNode true
                }
            }
            steps {
                sh '''
              npm install -g netlify-cli
              netflify --version
              echo "Deploying to Netlify site ID: $NETLIFY_SITE_ID"
              netlify deploy --dir=build --prod --site=$NETLIFY_SITE_ID
                '''
                // Add your build steps here
            }
        }
        stage('Start') {
            steps {
                echo 'npm run start'
                // Add your notification steps here
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages.'
            junit 'test-results/**/*.xml'
        }
        success {
            echo 'This will run only if the pipeline succeeds.'
        }
        failure {
            echo 'This will run only if the pipeline fails.'
        }
    }
}