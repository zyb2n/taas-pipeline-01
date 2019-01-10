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
  - name: busybox
    image: busybox
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('check inspec') {
      steps {
        container('taas') {
          sh 'inspec version'
        }
        container('busybox') {
          sh 'hostname -i',
          sh '/bin/busybox'
        }
      }
    }
  }
}
