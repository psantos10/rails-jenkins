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

    stage('Installing dependencies') {
      agent {
        label 'rails'
      }

      steps {
        xingRubyVersion(rubyVersionFile: '.ruby-version', sharedEnvironment: false) {\
          sh "gem install bundler"
          sh "bundle install --jobs=4"
        }
      }
    }

    stage('Test') {
      agent {
        label 'rails'
      }

      steps {
        xingRubyVersion(rubyVersionFile: '.ruby-version', sharedEnvironment: false) {
          sh "bundle exec rspec"
          step([$class: 'GitHubCommitStatusSetter'])
        }
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
  }
}
