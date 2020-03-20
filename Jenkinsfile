pipeline{
agent any
tools{
jdk 'java'
maven 'maven'
}
options{
buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '5'))
timestamps()
}
stages{
stage('show the dir'){
steps{
sh '''
echo "this is pipe line project"
echo "PATH=${PATH}"
'''
}
}
stage('checkout code from the git'){
steps{
git branch: 'ho', changelog: false, credentialsId: 'git', poll: false, url: 'https://github.com/narendrakumar998931/new.git'
}
}
stage('project build through maven'){
steps{
sh 'mvn clean install package'
}
}
stage('create artifact'){
steps{
archiveArtifacts '**/*.war'
}
}
}
}
