pipeline {
    agent any

    environment {
        DOCKER_HOST = 'tcp://host.docker.internal:2375'
        DOCKER_TLS_VERIFY = '0'
        DOCKER_CERT_PATH = ''
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    echo "Docker info:"
                    docker version

                    echo "Pulling node image"
                    docker pull node:18-alpine

                    echo "Running container to build"
                    docker run --rm -v $PWD:/app -w /app node:18-alpine sh -c "
                        ls -la
                        node --version
                        npm --version
                        npm ci
                        npm run build
                        ls -la
                    "
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    echo "Running tests in node container"
                    docker run --rm -v $PWD:/app -w /app node:18-alpine sh -c "
                        test -f build/index.html
                        npm test
                    "
                '''
            }
        }
    }
}
