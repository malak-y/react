pipeline {
  agent { label 'docker' }

  options {
    timestamps()
  }

  environment {
    DOCKER_BUILDKIT = '0'
  }

  stages {
    stage('Build docker image') {
      steps {
        wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'xterm']) {
          sh '''#!/bin/bash
            set -Eeuxo pipefail
            docker version
            docker build --progress=plain \
              -t malak1782003/docker-reco \
              -f Dockerfile .
          '''
        }
        archiveArtifacts artifacts: 'build.log', allowEmptyArchive: true
      }
    }

    stage('RUN TESTS') {
      steps {
        wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'xterm']) {
          sh '''#!/bin/bash
            set -Eeuxo pipefail
            docker run --rm -e CI=true \
              malak1782003/docker-reco \
              bash -lc 'set -Eeuxo pipefail; npm --loglevel verbose run test -- --ci --watchAll=false'
          '''
        }
        archiveArtifacts artifacts: 'test.log', allowEmptyArchive: true
      }
    }
  }
}
