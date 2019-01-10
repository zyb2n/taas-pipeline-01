@NonCPS
def loop_of_sh(list) {
    list.each { item ->
        sh "echo Host: ${item}"
        sh "inspec exec /tmp/taas-pipeline-01/ec2-linux/controls/ -t ssh://ec2-user@${item} --reporter cli json:$BUILD_NUMBER/json/${item}.output.json junit:$BUILD_NUMBER/junitreport/${item}.junit.xml html:$BUILD_NUMBER/www/${item}.index.html || true"
    }
}

hosts = ['10.2.1.234', '10.2.6.149', '10.2.4.27']

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
    image: zyb2n/taastest:1.1
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('build') {
      steps {
        container('taas') {
          sshagent (credentials: ['ssh-kenzan-scratch']) {
            sh 'inspec version'
	    sh 'git clone https://github.com/zyb2n/taas-pipeline-01.git /tmp/taas-pipeline-01'
		loop_of_sh(hosts)
         }
	archiveArtifacts artifacts: '$BUILD_NUMBER/*/*', fingerprint: true
        }
      }
   post {
  always {
    junit "$BUILD_NUMBER/junitreport/*.xml"
  }

    }

}
  }

}
