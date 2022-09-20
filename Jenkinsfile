def args = [:]
args.CLUSTER_NAME = "https://172.30.0.1"
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
                                def build = openshift.selector("bc", "${args.SERVICE_NAME}").exists()
                                startedBuild = build.startBuild()
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