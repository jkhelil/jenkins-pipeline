pipeline {
  agent { node { label 'ose-jenkins-agent-maven' } }
  stages {
    stage('raw') {
      steps {
        script {
         node("ose-jenkins-agent-maven") {
	  sh 'mvn --version'         
}
        }
      }
    }
}
}
