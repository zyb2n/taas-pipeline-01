pipeline {
  agent {
    kubernetes {
      label 'taaspod'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: taas-jenkins-slave
spec:
  containers:
  - name: taas
    image: zyb2n/taastest:latest
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('Create build output') {
      steps {
        container('taas') {
          sshagent (credentials: ['ssh-kenzan-scratch']) {
            sh 'inspec version'
            sh 'ssh -o StrictHostKeyChecking=no ec2-user@10.2.1.234 uname -a'
         }
        }
      }
    }
  }
}
