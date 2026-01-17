pipeline {
    agent any

    tools {
        // This will now work once the plugin is installed
        nodejs 'node18' 
    }

    stages {
        stage('Build') {
            steps {
                // On Windows, use 'bat' instead of 'sh' if your Jenkins agent is Windows
                // But if you are inside a bash-like shell (Git Bash), 'sh' might work.
                // I'll use 'sh' here assuming you are using a Linux-based Jenkins controller.
                sh '''
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
    }

    post {
        always {
            junit testResults: 'test-results/junit.xml', allowEmptyResults: true
        }
    }
}