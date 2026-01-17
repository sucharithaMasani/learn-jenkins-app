pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    withEnv([
                        'DOCKER_HOST=tcp://host.docker.internal:2375',
                        'DOCKER_TLS_VERIFY=0',
                        'DOCKER_CERT_PATH='
                    ]) {
                        docker.image('node:18-alpine').inside {
                            sh '''
                                echo "Docker inside container:"
                                docker version
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
                    withEnv([
                        'DOCKER_HOST=tcp://host.docker.internal:2375',
                        'DOCKER_TLS_VERIFY=0',
                        'DOCKER_CERT_PATH='
                    ]) {
                        docker.image('node:18-alpine').inside {
                            sh '''
                                echo "Docker inside container:"
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

