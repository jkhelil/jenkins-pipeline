podTemplate(cloud: 'openshift', name: 'maven') {
  node(POD_LABEL) {
    stage('test cat') {
      container('jnlp') {
        sh 'mvn --version'
        sh 'export JAVA_HOME=/usr/lib/jvm//java-11-openjdk'
        sh 'mvn clean package -X'
      }
    }
  }
}
