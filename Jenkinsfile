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
   sh 'docker build -t rajnikhattarrsinha/javamaven:2.0.0 .'
   }  
   
   stage('Publish Docker Image')
   {
      withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
           sh "docker login -u rajnikhattarrsinha -p ${dockerPWD}"
         }
      sh 'docker push rajnikhattarrsinha/javamaven:2.0.0'
   }

   stage('Pull Docker Image and Deploy'){
      
   /*
      // def dockerStop= "sudo docker stop $(docker ps -a)"
      def dockerContainersList='docker ps -a -q'
      //def dockerStop= 'sudo docker stop $(dockerContainersList)' 
       sshagent(['dockerdeployserver2']) {
          //sh "ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236 sudo docker stop ${dockerContainersList}"
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236'
          //sh 'docker pa-a -q'
          sh 'sudo docker stop $("$({dockerContainersList})")'
          //docker stop $(docker ps -a -q)

       }
        */
         
      sshagent(['dockerdeployserver2']) {
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236'
         sh 'sudo su'
         sh './stopscript.sh'
        // sh 'cd /home/ubuntu/script'
        // sh './stopcontainer.sh'
      } 
     
      def dockerContainerName = 'javatommav-$BUILD_NUMBER'
      def dockerRun= "sudo docker run -p 8080:8080 -d --name ${dockerContainerName} rajnikhattarrsinha/javamaven:2.0.0"
        sshagent(['dockerdeployserver2']) {
        sh "ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236 ${dockerRun}"
         //sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236'
        // sh "docker ps -f name=${dockerContainerTobeDeleted} -q | xargs --no-run-if-empty docker container stop"
        // sh "docker container ls -a -fname=${dockerContainerTobeDeleted} -q | xargs -r docker container rm"
         //sh 'sudo docker stop "${(docker ps -a)}"'
         //sh 'docker rm `docker ps --all`'
        // sh "docker stop ${dockerContainerTobeDeleted}"
         //sh "${dockerRun}"         
      }
   }   
}


