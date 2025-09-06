pipeline {
  agent { label 'docker' }

  options {
    timestamps()   // add times to each log line
  }

  environment {
    DOCKER_BUILDKIT = '1' // enable BuildKit
  }

  stages {
    stage('Build docker image') {
      steps {
        wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'xterm']) {
          sh '''
            set -Eeuxo pipefail
            docker version
            docker build --progress=plain \
              -t malak1782003/docker-reco \
              -f Dockerfile .
          ''' | tee build.log
        }
      }
    }

    stage('RUN TESTS') {
      steps {
        wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'xterm']) {
          sh '''
            set -Eeuxo pipefail
            docker run --rm -e CI=true \
              malak1782003/docker-reco \
              bash -lc 'set -Eeuxo pipefail; npm --loglevel verbose run test -- --ci --watchAll=false'
          ''' | tee test.log
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '*.log', allowEmptyArchive: true
    }
  }
}
