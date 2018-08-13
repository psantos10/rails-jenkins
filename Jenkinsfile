pipeline {
  agent none

  triggers {
    githubPush()
  }

  void setBuildStatus(String message, String state) {
    step([
        $class: "GitHubCommitStatusSetter",
        reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://source.xing.com/patricio-dossantos/rails-jenkins"],
        contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
        errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
        statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
    ]);
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
