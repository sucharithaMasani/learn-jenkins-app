pipeline {
    agent any

    // Defining tools allows Jenkins to install Node automatically if Docker fails
    tools {
        nodejs 'node18' // This name must match what you set in Global Tool Configuration
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    // Added args to help with Windows/Linux file permission mapping
                    args '-u root' 
                }
            }
            steps {
                sh '''
                    npm ci
                    npm run build
                '''
                // SAVE the build folder so the next stage can see it
                stash name: 'build-assets', includes: 'build/**'
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u root'
                }
            }
            steps {
                // BRING BACK the build folder from the previous stage
                unstash 'build-assets'
                
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
    }

    post {
        always {
            // allowEmptyResults: true prevents the pipeline from failing 
            // if tests didn't run due to a build error
            junit testResults: 'test-results/junit.xml', allowEmptyResults: true
        }
    }
}