def getCommitSha() {
  sh "git rev-parse HEAD > .git/current-commit"
  return readFile(".git/current-commit").trim()
}

def getRepoURL() {
  sh "git config --get remote.origin.url > .git/remote-url"
  return readFile(".git/remote-url").trim()
}

def setBuildStatus(message, state) {
  repoUrl = getRepoURL()
  commitSha = getCommitSha()
  step([
      $class: "GitHubCommitStatusSetter",
      commitShaSource: [$class: "ManuallyEnteredShaSource", sha: commitSha],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: 'continuous-integration/jenkins/tests'],
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: repoUrl],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

node('rails') {
  currentBuild.result = "SUCCESS"
  try {
      stage('Checkout'){
        checkout scm
        setBuildStatus('Checking out code', 'PENDING')
      }

      stage('Test'){
        setBuildStatus('Running tests', 'PENDING')
        // sh './scripts/jenkins/docker-run.sh /bin/bash -c "./scripts/run-tests.sh"'
        echo "Testing..."
      }
  }
  catch (err) {
      currentBuild.result = "FAILURE"
      throw err
  } finally {
    if (currentBuild.result == "SUCCESS") {
      setBuildStatus('All good!', "SUCCESS")
    } else {
      setBuildStatus('Tests are failing!', "FAILURE")
    }
  }
}
