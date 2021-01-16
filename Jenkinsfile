node ('docker-agent') {
    
    def buildNumber = BUILD_NUMBER
    def mavenHome= tool name: "maven", type: "maven"
    
    stage ("Git clone") {
        
        git url: "https://github.com/repo01/maven-web-application.git", brach: "master"
    }
    
    stage("Maven Package") {
        
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage("Creation of Docker Image") {
        
        sh "docker build -t geethika609/hello-world:${buildNumber} ."
    }
    
    stage("Docker login and push") {
        
        withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'DockerHubPwd')]) {
            sh "docker login -u geethika609 -p ${DockerHubPwd}"
        }
        sh "docker push geethika609/hello-world:${buildNumber}"
    }
    
    stage("Deploy Application as Docker container in Docker Deployment Server") {
        
        sshagent(['Docker_Server_SSH']) {
            
            sh "ssh -o StrictHostKeyChecking=no ubuntu@13.233.178.191 docker rm -f helloworld || true"
            sh "ssh -o StrictHostKeyChecking=no ubuntu@13.233.178.191 docker run -d -p 8080:8080 --name helloworld geethika609/hello-world:${buildNumber}"
        }
    }
    
}
