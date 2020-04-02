pipeline {
    agent any
    options {
        checkoutToSubdirectory('argo-ncg')
    }
    environment {
        PROJECT_DIR="argo-ncg"
        GIT_COMMIT=sh(script: "cd ${WORKSPACE}/$PROJECT_DIR && git log -1 --format=\"%H\"",returnStdout: true).trim()
        GIT_COMMIT_HASH=sh(script: "cd ${WORKSPACE}/$PROJECT_DIR && git log -1 --format=\"%H\" | cut -c1-7",returnStdout: true).trim()
        GIT_COMMIT_DATE=sh(script: "date -d \"\$(cd ${WORKSPACE}/$PROJECT_DIR && git show -s --format=%ci ${GIT_COMMIT_HASH})\" \"+%Y%m%d%H%M%S\"",returnStdout: true).trim()

    }
    stages {
        stage ('Centos 7') {
            agent {
                docker {
                    image 'argo.registry:5000/epel-7-mgo'
                    args '-u jenkins:jenkins'
                }
            }
            steps {
                echo 'Building Rpm...'
                withCredentials(bindings: [sshUserPrivateKey(credentialsId: 'jenkins-rpm-repo', usernameVariable: 'REPOUSER', \
                                                            keyFileVariable: 'REPOKEY')]) {
                    sh "/home/jenkins/build-rpm.sh -w ${WORKSPACE} -b ${BRANCH_NAME} -d centos7 -p ${PROJECT_DIR} -s ${REPOKEY}"
                }
                archiveArtifacts artifacts: '**/*.rpm', fingerprint: true
            }
        }
        stage ('Centos 6') {
            agent {
                docker {
                    image 'argo.registry:5000/epel-6'
                    args '-u jenkins:jenkins'
                }
            }
            steps {
                echo 'Building Rpm...'
                withCredentials(bindings: [sshUserPrivateKey(credentialsId: 'jenkins-rpm-repo', usernameVariable: 'REPOUSER', \
                                                             keyFileVariable: 'REPOKEY')]) {
                    sh "/home/jenkins/build-rpm.sh -w ${WORKSPACE} -b ${BRANCH_NAME} -d centos6 -p ${PROJECT_DIR} -s ${REPOKEY}"
                }
                archiveArtifacts artifacts: '**/*.rpm', fingerprint: true
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}