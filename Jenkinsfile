podTemplate(cloud: 'openshift', yaml:'''
spec:
  containers:
  - name: jnlp
    image: jenkins/jnlp-slave:4.0.1-1
    volumeMounts:
    - name: home-volume
      mountPath: /home/jenkins
    env:
    - name: HOME
      value: /home/jenkins
  - name: maven
    image: registry.redhat.io/openshift4/ose-jenkins-agent-maven:v4.4.0
    command: ['cat']
    tty: true
    volumeMounts:
    - name: home-volume
      mountPath: /home/jenkins
    env:
    - name: HOME
      value: /home/jenkins
    - name: MAVEN_OPTS
      value: -Duser.home=/home/jenkins
  imagePullSecrets:
      - name: 6442445-jawed-pull-secret
  volumes:
  - name: home-volume
    emptyDir: {}
''') {
  node(POD_LABEL) {
    stage('Build a Maven project') {
      container('maven') {
        git branch: '1839322', url: 'https://github.com/jkhelil/jenkins-pipeline.git'
        sh 'export JAVA_HOME=/usr/lib/jvm//java-11-openjdk && export /opt/rh/rh-maven35/root/usr/bin/mvn clean package -X'
      }
    }
  }
}
