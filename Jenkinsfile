pipeline {
  agent any
  options {
    timestamps();
    copyArtifactPermission('toprepo');
  }

  stages {

    stage('checkout') {
      steps {
        checkout scm
      }
    }

    stage('setup') {
      steps {
        sh """
          autoreconf --verbose --install --force
          ./configure
        """
      }
    }

    stage('build') {
      steps {
        sh """
          make all
          make dist
          make package
        """
      }
    }
    stage('who-am-i') {
      steps {
        echo 'tom branch'
        echo 'branch ...' + env.BRANCH_NAME
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: 'sub*.deb', onlyIfSuccessful: true
      step([$class: 'WsCleanup'])
    }
  }
}

