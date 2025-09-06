pipeline {
  agent { label 'docker' }

  options {
    timestamps()                 // add times to each log line
    ansiColor('xterm')           // nicer colored logs
  }

  environment {
    DOCKER_BUILDKIT = '1'        // enable BuildKit
  }

  stages {
    stage('Build docker image') {
      steps {
        // -e: fail fast, -x: echo each command, -o pipefail: catch pipe errors
        sh label: 'Docker Build (verbose)', script: '''
          set -Eeuxo pipefail
          docker version
          docker build --progress=plain \
            -t malak1782003/docker-reco \
            -f Dockerfile .
        ''' | tee build.log
      }
    }

    stage('RUN TESTS') {
      steps {
        sh label: 'Run tests inside container (verbose)', script: '''
          set -Eeuxo pipefail
          # pass CI=true to tools like CRA/Jest
          docker run --rm -e CI=true \
            malak1782003/docker-reco \
            bash -lc 'set -Eeuxo pipefail; npm --loglevel verbose run test -- --ci --watchAll=false'
        ''' | tee test.log
      }
    }
  }

  post {
    always {
      // keep logs with the build so you can download them
      archiveArtifacts artifacts: '*.log', allowEmptyArchive: true
    }
  }
}
