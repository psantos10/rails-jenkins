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
        step([$class: 'GitHubCommitStatusSetter'])
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
  }
}
