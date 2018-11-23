node{
   stage('SCM Checkout'){
     git 'https://github.com/rajnikhattarrsinha/java-tomcat-maven-example'
   }
   stage('Mvn Package'){
      // Get maven home path
      def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"
   }
   stage('Build Docker Image'){
   sh 'docker build -t rajnikhattarrsinha/java-tomcat-maven-example:2.0.0 .'
   }
   stage('Push Docker Image')
   {
      withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
    // some block
         sh "docker login -u rajnikhattarrsinha -p ${dockerPWD}"
         }
      sh 'docker push rajnikhattarrsinha/java-tomcat-maven-example:2.0.0'
   
   }
   
   stage('Run Container on Deployment-server'){
      def dockerRun= 'sudo docker run -p 8080:8080 -d --name my-app rajnikhattarrsinha/java-tomcat-maven-example:2.0.0'
      sshagent(['dockerdeployserver2']) {
    // some block
         sh "ssh -o StrictHostKeyChecking=no ubuntu@54.174.128.73 ${dockerRun}"
      }
   }
   
   /*stage('Run Container on Deployment-server'){
      def dockerRun= 'docker run -p 8080:8080 -d --name my-app rajnikhattarrsinha/my-app:2.0.0'
      sshagent(['deployserver']) {
    // some block
         sh "ssh -o StrictHostKeyChecking=no root@54.144.60.78 ${dockerRun}"
      }
   }
   */
   
   
}


