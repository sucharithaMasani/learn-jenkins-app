pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Connect to Docker TCP without TLS
                    docker.withServer('tcp://192.168.5.71:2375', '') {
                        docker.image('node:18-alpine').inside {
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
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.withServer('tcp://192.168.5.71:2375', '') {
                        docker.image('node:18-alpine').inside {
                            sh '''
                                test -f build/index.html
                                npm test
                            '''
                        }
                    }
                }
            }
        }
    }
}
