pipeline() {
agent any
stages() {
stage("git init") {
steps{
checkout changelog: false, scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/naveenandukuri/jenkins-docker-project.git']])
}
}
stage("docker image build"){
steps{
sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
sh 'docker image tag $JOB_NAME:v1.$BUILD_ID naveenandukuri/$JOB_NAME:v1.$BUILD_ID'
sh 'docker image tag $JOB_NAME:v1.$BUILD_ID naveenandukuri/$JOB_NAME:latest'
}
}
}
}
