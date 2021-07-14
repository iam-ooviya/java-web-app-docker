node
{
    def buildNumber = BUILD_NUMBER
      stage('SCM Checkout'){
        git url: 'https://github.com/iam-ooviya/java-web-app-docker.git',branch: 'master'
    }
     stage(" Maven Clean Package"){
      def mavenHome = tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
 stage("Build Docker Image"){
        sh "docker build -t iamooviya/webapps:${buildNumber} ."
    }
        stage("Docker login and Push"){
        withCredentials([string(credentialsId: 'docker_hub_p', variable: 'docker_hub_p')]){
         sh "docker login -u iamooviya -p ${Docker_hub_p} " 
           }
        sh "docker push iamooviya/webapps:${buildNumber}"
    }
        stage("Deploy to dockercontinor in docker deployer"){
        sshagent(['docker_ssh_password']) {
            sh "ssh -o StrictHostKeyChecking=no ubuntu@13.232.125.1 docker rm -f cloudcandy || true"
            sh "ssh -o StrictHostKeyChecking=no ubuntu@13.232.125.1 docker run -d -p 9000:8080 --name cloudcandy iamooviya/webapps:${buildNumber}"           
    }  
}

}
