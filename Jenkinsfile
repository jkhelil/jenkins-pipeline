pipeline {
  agent { node { label 'maven' } }
  stages {
    stage('raw') {
      steps {
        script {
         node("maven") {
          git branch: '1839322', url: 'https://github.com/jkhelil/jenkins-pipeline.git'
          
	  sh '''
            git clone https://github.com/jkhelil/jenkins-pipeline.git && true
            cd /tmp/workspace/jawed/jawed-jenkins-pipeline && true
            mvn clean package -X && true
            sleep 10000
          '''         
}
        }
      }
    }
}
}
