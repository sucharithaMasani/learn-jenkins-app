pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // We use 'bat' because you are on Windows
                bat '''
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Test') {
            steps {
                bat '''
                    if exist build\\index.html (echo Build exists) else (exit 1)
                    npm test
                '''
            }
        }
    }

    post {
        always {
            // Added allowEmptyResults so the pipeline doesn't crash if tests fail
            junit testResults: '**/junit.xml', allowEmptyResults: true
        }
    }
}