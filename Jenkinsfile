node{
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
            scp target/maven-web-application.war ec2-user@54.145.19.193:/opt/tomcat/webapps
            """

            //sh "scp target/maven-web-application.war ec2-user@54.145.19.193:/opt/tomcat/webapps"
        }
    }
}
