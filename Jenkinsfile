pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/beomtaek78/jenkinstest.git', branch: 'main'
      }
    }
    stage('docker build and push') {
      steps {
        sh '''
        sudo docker build -t brian24/keduitlab:red .
        sudo docker push brian24/keduitlab:red
        ansible node -m command -a "sudo docker pull brian24/keduitlab:red"
        '''
      }
    }
    stage('deploy and service') {
      steps {
        sh '''
        ansible master -m command -a 'sudo kubectl create deploy web-red --replicas=3 --port=80 --image=brian24/keduitlab:red'
        ansible master -m command -a 'sudo kubectl expose deploy web-red --type=LoadBalancer --port=80 --target-port=80 --name=web-red-svc'
        '''
      }
    }
  }
}
