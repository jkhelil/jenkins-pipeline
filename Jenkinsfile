def args = [:]
args.CLUSTER_NAME = "https://api.daily-4.11-092008.dev.openshiftappsvc.org:6443"
args.PROJECT_NAME = "jawed1"
args.SERVICE_NAME = "jenkins-pipeline"
args.SERVICE_VERSION = "0.0.1-SNAPSHOT"

pipeline {
    agent any
        options {
            disableConcurrentBuilds()
            buildDiscarder(logRotator(numToKeepStr: '5'))
        }
        stages {
            stage('Preamble') {
                steps {
                    script {
                        openshift.withCluster() {
                            openshift.withProject() {
                                echo "Using Project: ${openshift.project()}"
                                echo "Using Cluster: ${openshift.cluster()}"
                            }
                        }
                    }
                }
            }
            stage ('Code Checkout') {
                steps {
                    script {
                        checkout scm
                    }
                }
            }
            stage('start build') {
                steps {
                    script {
                        openshift.withCluster() {
                            openshift.withProject(args.PROJECT_NAME) {
                                //openshift.newBuild("--name=jawed-eap", "--image-stream=eap-app")

                                def buildConfig = openshift.selector("bc", "eap-app-build-artifacts")
                                def startedBuild = buildConfig.startBuild("")
                                // Wait and watch logs
                                startedBuild.logs("-f")
                                
                                // Check status
                                def status = startedBuild.object().status
                                echo "build status: ${startedBuild.object().status}";
                                //def builds = buildConfig.related('builds')
                                // But we can actually want to wait for the build to complete.
                                startedBuild.watch {
                                    if ( it.count() == 0 ) return false
                                    // A robust script should not assume that only one build has been created, so
                                    // we will need to iterate through all builds.
                                    def allDone = true
                                    it.withEach {
                                        // 'it' is now bound to a Selector selecting a single object for this iteration.
                                        // Let's model it in Groovy to check its status.
                                        def buildModel = it.object()
                                        if ( it.object().status.phase != "Complete" ) {
                                            allDone = false
                                        }
                                    }
                                    return allDone;
                                }
                                
                                
                                //assert status.phase == "Complete"
                            }
                        }
                    }
                }   
            }
        }
}
