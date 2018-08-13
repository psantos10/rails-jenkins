pipeline {
  agent none

  triggers {
    githubPush()
  }

  stages {
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }

    stage('Test') {
      agent {
        label 'rails'
      }

      steps {
        echo 'Testing...'
        step([$class: 'GitHubSetCommitStatusBuilder'])
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
  }

  post {
    success {
        setBuildStatus("Build succeeded", "SUCCESS");
    }
    failure {
        setBuildStatus("Build failed", "FAILURE");
    }
  }
}
