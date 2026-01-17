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
                // Force Docker CLI from path, ignore plugin
                sh '''
                    echo "Docker version:"
                    /usr/bin/docker version || docker version

                    echo "Pull node image"
                    /usr/bin/docker pull node:18-alpine || docker pull node:18-alpine

                    echo "Run build container"
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
                    echo "Run tests container"
                    docker run --rm -v $PWD:/app -w /app node:18-alpine sh -c "
                        test -f build/index.html
                        npm test
                    "
                '''
            }
        }
    }
}
