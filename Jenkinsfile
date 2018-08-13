pipeline {
  agent none

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
}
