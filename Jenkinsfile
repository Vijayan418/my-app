node{
   def buildNumber = BUILD_NUMBER
   stage('Git-Checkout'){
     git 'https://github.com/Vijayan418/my-app.git'
   }
   
   
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
  
  
   stage('Build Docker Imager'){
   sh "docker build -t vijayan418/myweb:${buildNumber} ."
   }
   
   
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u vijayan418 -p ${dockerPassword}"
    }
   sh "docker push vijayan418/myweb:${buildNumber}"
   }
   
   
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
	
	
   stage('Docker Deployment'){
   sh "docker run -d -p 8090:8080 --name tomcattest vijayan418/myweb:${buildNumber}" 
   }
   
   
   stage('Docker Image Remove '){
   sh 'docker rmi $(docker images -a -q) || true'
   }
   
}
}
