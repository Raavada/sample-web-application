def getDockerTag() {
 def tag = sh script: 'git rev-parse HEAD', returnStdout: true 
 return tag
}
pipeline{

      agent {
                docker {
                image 'maven'
                args '-v $HOME/.m2:/root/.m2'
                }
             }
      environment {
          Docker_tag = getDockerTag()
      }
        
        stages{

              stage('Quality Gate Status Check'){
                  steps{
                      script{
			      
		    	    sh "mvn clean install"
		  
                 	}
               	 }  
              }	

              stage('build'){
		      steps {
			      script{
                sh 'docker build . -t raavada/devops-training:$Docker_tag'                
		withCredentials([string(credentialsId: 'docker', variable: 'docker_password')]) {
    
                sh '''docker login -u raavada -p $docker_password
                docker push raavada/devops-training:$Docker_tag
		'''
                }
                
			      }
		      }
              }
		
            }	       	     	         
}
