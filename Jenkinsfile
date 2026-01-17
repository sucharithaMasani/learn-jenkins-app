pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Use 'sh' for Linux/Unix, not 'bat'
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
                    test -f build/index.html && echo "Build exists" || exit 1
                    npm test
                '''
            }
        }
    }
}