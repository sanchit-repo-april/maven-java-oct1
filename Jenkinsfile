node{
    echo "here abhishek is $BUILD_NUMBER"
    def mvnHome = tool name: '3.9.9' 
    // cloning git repo
    stage('checkout'){
        git 'https://github.com/sanchit-repo-april/maven-java-oct1.git'
    }
    
    // creating war/build file
    stage('build'){
        sh "$mvnHome/bin/mvn clean package"
    }
    
    // sonarqube report
    stage('sonarreport'){
        sh "$mvnHome/bin/mvn sonar:sonar"
    }
    // upload to nexus
    stage('uploadTONexus'){
        sh "$mvnHome/bin/mvn deploy"
    }
    // upload to tomcat
    stage('deploying to tomcat'){
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
