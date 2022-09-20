def args = [:]
args.CLUSTER_NAME = "https://api.daily-4.11-092008.dev.openshiftappsvc.org:6443"
args.PROJECT_NAME = "jawed"
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
                        openshift.withCluster(args.CLUSTER_NAME) {
                            openshift.withProject(args.PROJECT_NAME) {
                                def buildConfig = openshift.selector("bc", "${args.SERVICE_NAME}").exists()
                                startedBuild = buildConfig.startBuild("jenkins-pipeline")
                                // Wait and watch logs
                                startedBuild.logs("-f")
                                // Check status
                                def status = startedBuild.object().status
                                echo "build status: ${startedBuild.object().status}";
                                assert status.phase == "Complete"
                            }
                        }
                    }
                }   
            }
        }
}