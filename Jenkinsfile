pipeline {
  agent any

  environment {
    GIT_COMMIT_SHORT = sh(returnStdout: true, script: '''echo $GIT_COMMIT | head -c 7''')
  }

  stages {
    stage('Prepare .env') {
      steps {
        sh 'echo GIT_COMMIT_SHORT=$(echo $GIT_COMMIT_SHORT) > .env'
      }
    }

    stage('Build database') {
      steps {
        dir('database') {
          sh 'docker build . -t cilist-pipeline-db:$GIT_COMMIT_SHORT'
          sh 'docker tag cilist-pipeline-db:$GIT_COMMIT_SHORT profesorgreen36/cilist-pipeline-db:$GIT_COMMIT_SHORT'
        }
      }
    }

    stage('Build backend') {
      steps {
        dir('backend') {
          sh 'docker build . -t cilist-pipeline-be:$GIT_COMMIT_SHORT'
          sh 'docker tag cilist-pipeline-be:$GIT_COMMIT_SHORT profesorgreen36/cilist-pipeline-be:$GIT_COMMIT_SHORT'
        }
      }
    }
    
    stage('Build frontend') {
      steps {
        dir('frontend') {
          sh 'docker build . -t cilist-pipeline-fe:$GIT_COMMIT_SHORT'
          sh 'docker tag cilist-pipeline-fe:$GIT_COMMIT_SHORT profesorgreen36/cilist-pipeline-fe:$GIT_COMMIT_SHORT'
        }
      }
    }
  }
}
