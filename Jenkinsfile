pipeline{
    agent any
    
    tools{
        maven "3.9.9"
    }
    options {
                buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '4')
             }
    stages{
        stage('cloning'){
            steps{
                git 'https://github.com/sanchit-repo-april/maven-java-oct1.git'
            }
        }
        stage('building'){
            steps{
                sh "mvn clean package"
            }
        }
        stage('quality analysis to sonar'){
            steps{
                sh "mvn sonar:sonar"
            }
        }
        stage('uploading to nexus'){
            steps{
                sh "mvn deploy"
            }
        }
        stage('tomcat'){
            steps{
                sshagent(['tomcatsshkey']) {
                     sh """
                      # Copy WAR to a directory ec2-user can write to
                        scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null \
                         target/maven-web-application.war ec2-user@54.145.19.193:/home/ec2-user/

                      # SSH and move the WAR file using sudo
                         ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ec2-user@54.145.19.193 \\
                        'sudo mv /home/ec2-user/maven-web-application.war /opt/tomcat/webapps/'
                     """
    }
            }
        }
        
    }
}
