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
        echo 'branch ..' + env.BRANCH_NAME
      }
    }
    stage('merge') {
      when {
        branch 'main'
      }
      steps {
        sh """
          echo this is the main branch
        """
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

