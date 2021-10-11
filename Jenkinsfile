pipeline{
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('code pull frm git '){
            steps{
                git branch: 'master', url: 'https://github.com/narendrakumar998931/pipeline.git' 
            }
        }
        stage('build the code'){
            steps{
                sh 'mvn clean install -DskipUnitTests'
            }
        }
        stage('test the code'){
            steps{
                sh 'mvn test'
            }
        }
        stage('package the code'){
            steps{
                sh 'mvn package'
            }
        }
        stage('quality test useing checkstyle'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('quality analysis with sonarquib'){
            environment{
                scannerHome = tool 'sonar'
            }
            steps{
                withSonarQubeEnv('paac-sonar') {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=git \
                   -Dsonar.projectName=git \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
              
            }
        }
        stage('upload artifact to nexus'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'web', classifier: '', file: 'target/web.war', type: 'war']], credentialsId: 'nexus', groupId: 'nani', nexusUrl: '10.1.2.53:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'git-repo', version: '1.0'
            }
        }
        stage('deploy to tomcat'){
            steps{
                script{
                    sh("cp  /var/lib/jenkins/workspace/git-pipeline/target/web.war /opt/apache-tomcat-9.0.54/webapps ")

                }
            }
        }
    }
}
