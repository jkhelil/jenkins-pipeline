pipeline {
  agent { node { label 'maven' } }
  stages {
    stage('raw') {
      steps {
        script {
         node("maven") {
          git branch: '1839322', url: 'https://github.com/jkhelil/jenkins-pipeline.git'
	  sh '''
            cd jenkins-pipeline
            mvn clean package -X
          '''         
}
        }
      }
    }
}
}
