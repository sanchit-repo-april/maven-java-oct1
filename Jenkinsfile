node{

    def mvnHome = tool name: 'maven 3.9.7', type: 'maven'
    stage('checkout/clone'){	
       git 'https://github.com/sanchit-repo-april/maven-java-oct1.git'	
    }

    stage('build'){	
         sh "$mvnHome/bin/mvn clean package"
    }
    stage('testing'){
        sh "$mvnHome/bin/mvn sonar:sonar"
    }
    stage('nexus'){
        sh "$mvnHome/bin/mvn deploy"
    }

    stage('deploying to tomcat'){
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
