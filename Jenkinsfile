node{
   stage('SCM Checkout'){
     git 'https://github.com/bharath2096/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('Build Docker Imager'){
   sh 'docker build -t bharath2096/myweb .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPs', variable: 'dockerPassword')]) {
   sh "docker login -u bharath2096 -p ${dockerPassword}"
    }
   sh 'docker push bharath2096/myweb'
   }
   stage('Remove Previous Container'){
        try{
                sh 'docker rm -f tomcattest'
        }catch(error){
                //  do nothing if there is an exception
        }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest bharath2096/myweb:0.0.2'
   }
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 13.233.83.129:8083"
   sh "docker tag saidamo/myweb:0.0.2 13.233.83.129:8083/damo:1.0.0"
   sh 'docker push 13.233.83.129:8083/damo:1.0.0'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
}
