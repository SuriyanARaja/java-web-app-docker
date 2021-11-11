node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/suriyanaraja/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t suriyanaraja/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker_hub_password', variable: 'Dockerpassword')]) {
          sh "docker login -u suriyanaraja -p ${Dockerpassword}"
        }
        sh 'docker push suriyanaraja/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app suriyanaraja/java-web-app'
         
         sshagent(['docker_ssh_password']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@13.234.59.178 docker stop java-web-app || true'
          sh 'ssh  ubuntu@13.234.59.178 docker rm java-web-app || true'
          sh 'ssh  ubuntu@13.234.59.178 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@13.234.59.178 ${dockerRun}"
       }
       
    }
     
     
}
