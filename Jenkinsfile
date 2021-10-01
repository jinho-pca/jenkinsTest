pipeline {
  agent any
  stages {
    stage('prepare') {
      steps {
        git(poll: true, url: 'https://github.com/happ-in/jenkinsTest.git', branch: 'main')
      }
    }

    stage('build') {
      steps {
        dir(path: 'test') {
          sh 'npm install'
          sh 'npm run build'
        }

      }
    }

  }
  tools {
    nodejs 'jenkins-node'
  }
}