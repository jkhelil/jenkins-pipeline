pipeline {
  agent { node { label 'maven' } }
  stages {
    stage('raw') {
      steps {
        script {
         node("maven") {
	  sh 'mvn clean package -X'         
}
        }
      }
    }
}
}
