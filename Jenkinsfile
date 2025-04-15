pipeline{
    agent any
    
    tools {
          maven 'maven 3.9.7'
    }
    stages{
        stage('checkout'){
            steps{
                git 'https://github.com/sanchit-repo-april/maven-java-oct1.git'
            }
        }
        stage('build'){
            steps{
                sh "mvn clean package"
            }
        }
        stage("sonar"){
            steps{
                sh "mvn sonar:sonar"
            }
        }
        stage("nexus"){
            steps{
                sh "mvn deploy"
            }
        }
        stage("tomcat"){
            steps{
                sshagent(['tomcatpipeline']) {
        sh """
        # Copy WAR to a directory ec2-user can write to
        scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null \
        target/maven-web-application.war ec2-user@54.226.153.176:/home/ec2-user/

        # SSH and move the WAR file using sudo
        ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ec2-user@54.226.153.176 \\
        'sudo mv /home/ec2-user/maven-web-application.war /opt/tomcat/webapps/'
        """
    }
            }
        }
    }
}
