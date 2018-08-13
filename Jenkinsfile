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

      xingRubyVersion(rubyVersionFile: '.ruby-version', sharedEnvironment: false) {\
        sh "gem install bundler"
        sh "bundle install --jobs=4"
      }
    }

    stage('Test') {
      agent {
        label 'rails'
      }
      
      xingRubyVersion(rubyVersionFile: '.ruby-version', sharedEnvironment: false) {
        sh "bundle exec rspec
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
  }
}
