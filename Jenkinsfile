pipeline {
  agent { node { label 'maven' } }
  stages {
    stage('raw') {
      steps {
        script {
         node("maven") {
          git branch: '1839322', url: 'https://github.com/jkhelil/jenkins-pipeline.git'
	  sh '''
            cd /tmp/workspace/jawed/jawed-jenkins-pipeline
            mvn clean package -X
          '''         
}
        }
      }
    }
}
}
