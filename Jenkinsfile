node{
      
      stage('Checkout'){
         git 'https://github.com/rajnikhattarrsinha/java-tomcat-maven-example'
      }
  
      stage('Build'){
         // Get maven home path and build
         def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
         sh "${mvnHome}/bin/mvn package"
      }
      /*
      stage ('Test'){
            sh 'make check || true' 
            junit '**/target/*.xml' 
      }
   
      stage ('Test'){
         def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'    
         sh "${mvnHome}/bin/mvn verify; sleep 3"
      }  
      */
    stage('Build Docker Image'){
         sh 'docker build -t rajnikhattarrsinha/javademo:2.0.0 .'
      }  
   
      stage('Publish Docker Image')
      {
         withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
              sh "docker login -u rajnikhattarrsinha -p ${dockerPWD}"
         }
        sh 'docker push rajnikhattarrsinha/javademo:2.0.0'
      }

   stage('Stop running containers'){        
         def listContainer='sudo docker ps'
         def scriptRunner='sudo ./stopscript.sh'
         def stopContainer='sudo docker stop $(docker ps -a -q)'
        sshagent(['dockerdeployserver2']) {
              sh "ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236 ${scriptRunner}"
            //sh "ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236 ${stopContainer}"
              //sh "ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236 ${dockerRun}"
            //sh '''ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236 ${stopContainer}'''
           // docker stop $(docker ps -a -q)
           // ${dockerRun}
           // '''
               //sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236'
              // sh "docker ps -f name=${dockerContainerTobeDeleted} -q | xargs --no-run-if-empty docker container stop"
              // sh "docker container ls -a -fname=${dockerContainerTobeDeleted} -q | xargs -r docker container rm"
               //sh 'sudo docker stop "${(docker ps -a)}"'
               //sh 'docker rm `docker ps --all`'
              // sh "docker stop ${dockerContainerTobeDeleted}"
               //sh "${dockerRun}"         
         }
   } 
      stage('Pull Docker Image and Deploy'){        
            def dockerContainerName = 'javademo-$BUILD_NUMBER'
            def dockerRun= "sudo docker run -p 8080:8080 -d --name ${dockerContainerName} rajnikhattarrsinha/javademo:2.0.0"         
            sshagent(['dockerdeployserver2']) {
              sh "ssh -o StrictHostKeyChecking=no ubuntu@18.215.68.236 ${dockerRun}"
                   
         }
   }
   
   
}


