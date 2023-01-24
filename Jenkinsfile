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
stage("docker push"){
steps{
        withCredentials([string(credentialsId: 'Dockerpassword', variable: 'Dockerpassword')]) {
        sh "docker login -u naveenandukuri -p ${Dockerpassword}"
        sh 'docker image push naveenandukuri/$JOB_NAME:latest'
        sh 'docker image push naveenandukuri/$JOB_NAME:v1.$BUILD_ID'
        sh "docker rmi -f `docker images -q`"
        }
}
}
stage("docker container deployment"){
steps{
sshagent(['ubuntu']) {
 sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.0.7 docker run -d -p 9000:80 --name scripted naveenandukuri/docker-deployment:latest'
}
}
}
}
}
