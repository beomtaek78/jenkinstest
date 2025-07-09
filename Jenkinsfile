pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/beomtaek78/jenkinstest.git', branch: 'main'
      }
    }
    stage('delivery and deployment') {
      steps {
        sh '''
        kubectl apply -f testpod.yml
        '''
      }
    }
  }
}
