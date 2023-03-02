pipeline {
  agent { node { label 'maven' } }  
  stages {
    stage('raw') {
      steps {
        script {
         node("maven") {
	  sh '''
            git clone -b 1839322 https://github.com/jkhelil/jenkins-pipeline.git && true
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
