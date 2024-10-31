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
        now=$(date +%y%m%d%H%M)
        sudo docker build -t brian24/keduitlab:${now} .
        sudo docker push brian24/keduitlab:${now}
        ansible node -m shell -a "sudo docker pull brian24/keduitlab:${now}"
        '''
      }
    }
    stage('deploy and service') {
      steps {
        sh '''
        ansible master -m shell -a 'sudo kubectl create deploy web-red --replicas=3 --port=80 --image=brian24/keduitlab:${now}'
        ansible master -m shell -a 'sudo kubectl expose deploy web-red --type=LoadBalancer --port=80 --target-port=80 --name=web-red-svc'
        '''
      }
    }
  }
}
