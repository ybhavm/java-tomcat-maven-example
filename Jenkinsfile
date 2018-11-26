node{
   stage('Checkout'){
      git 'https://github.com/rajnikhattarrsinha/java-tomcat-maven-example'
   }
   stage('Build'){
      // Get maven home path and build
      def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
      sh "${mvnHome}/bin/mvn install"
   }
   
   stage ('Test'){
      def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'    
      sh "${mvnHome}/bin/mvn verify; sleep 3"
   }  
   
   stage('Build Docker Image'){
   sh 'docker build -t rajnikhattarrsinha/javatomcat:2.0.0 .'
   }  
   
   stage('Publish Docker Image')
   {
      withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
           sh "docker login -u rajnikhattarrsinha -p ${dockerPWD}"
         }
      sh 'docker push rajnikhattarrsinha/javatomcat:2.0.0'
   }
   
   stage('Pull Docker Image and Deploy'){
      def previousBuildNo='$BUILD_NUMBER--'
      def dockerContainerName = 'java-tomcat-maven-$previousBuildNo'
      def dockerRun= "sudo docker run -p 8080:8080 -d --name ${dockerContainerName} rajnikhattarrsinha/javatomcat:2.0.0"
      //def dockerRun= 'sudo docker run -p 8080:8080 -d --name java-tomcat-maven-$BUILD_NUMBER rajnikhattarrsinha/javatomcat:2.0.0'

      sshagent(['dockerdeployserver2']) {
        //sh "ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236 ${dockerRun}"
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236'
         sh "docker ps -f name=${dockerContainerName} -q | xargs --no-run-if-empty docker container stop"
         sh "docker container ls -a -fname=${dockerContainerName} -q | xargs -r docker container rm"
         //sh 'sudo docker stop "${(docker ps -a)}"'
         sh "${dockerRun}"         
      }
   }   
}


