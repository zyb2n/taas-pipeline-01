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
    image: zyb2n/taastest:1.0
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
	    sh 'git clone 'https://https://github.com/zyb2n/taas-pipeline-01.git /tmp/'
            sh 'ssh -o StrictHostKeyChecking=no ec2-user@10.2.1.234 uname -a'
            sh "/usr/local/bin/inspec exec /tmp/ec2-linux/controls/ -t ssh://ec2-user@10.2.1.234 || true"
         }
        }
      }
    }
  }
}
