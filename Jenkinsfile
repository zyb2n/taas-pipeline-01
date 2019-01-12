def loop_of_sh(list) {
    for (int i = 0; i < list.size(); i++) {
        sh "inspec exec /tmp/taas-pipeline-01/ec2-linux/controls/ -t ssh://ec2-user@${list[i]} --reporter cli json:$BUILD_NUMBER/json/${list[i]}.output.json junit:$BUILD_NUMBER/junitreport/${list[i]}.junit.xml html:$BUILD_NUMBER/www/${list[i]}.index.html || true"
        sh "/es_loader.sh store-elasticsearch-client $BUILD_NUMBER/json/${list[i]}.output.json"
    }
}

hosts = ['10.2.6.149', '10.2.4.27']

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
    image: zyb2n/taastest:1.5
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
          sshagent (credentials: ['taas-ssh']) {
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
