pipeline{
agent any
tools{
java 'java'
maven 'maven'
}
options{
buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '5'))
timestamps()
}
stages{
stage('show the dir'){
steps{
sh'''
echo "this is pipe line project"
echo "PATH=$PATH"
}
}
stage{
steps('checkout code from the git'){
git branch: 'ho', changelog: false, credentialsId: 'git', poll: false, url: 'https://github.com/narendrakumar998931/new.git'
}
}
stage{
steps('project build through maven'){
sh 'mvn clean install package'
}
}
stage{
steps(create artifact){
archiveArtifacts '**/*.war'
}
}
}
}
