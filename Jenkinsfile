pipeline {
    agent {
        label 'docker'
    }
    
    stages {
        stage('Build docker image') {
            steps {
                sh 'docker build -t malak1782003/docker-reco -f Dockerfile.dev .'
            }
        }
        
        stage('RUN TESTS') {
            steps {
                script {
                    env.DOCKER_BUILDKIT = 1
                    sh 'docker run -e CI=true malak1782003/docker-reco npm run test'
                }
            }
        }
    }
}