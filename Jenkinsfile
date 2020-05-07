pipeline{
      
        agent {
                docker {
                image 'maven'
                args '-v $HOME/.m2:/root/.m2'
                }
	}	
             
			 stages{

              stage('Quality Gate Status Check'){
                  steps{
                      script{
                      withSonarQubeEnv('tmfsonarserver') { 
                      sh "mvn sonar:sonar"
                       }
		      sleep(60)
                      timeout(time: 10, unit: 'MINUTES') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
		    sh "mvn clean install"
                  }
                }  
              }



            }
	}		
