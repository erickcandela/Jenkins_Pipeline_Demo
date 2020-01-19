pipeline {
    agent any
	tools {
        maven 'MAVEN_CONF' 
    }   
     
    
    environment {
        EMAIL_TO = 'erick.candela@gmail.com'
    }
         
    stages {
        stage ('Compile Stage Clean') {
            steps {                
                    bat 'mvn clean'            	
        	}
		 	post {
				always {
					emailext body: 'Check console SC output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
					to: "${EMAIL_TO}", 
					subject: 'Build begin in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'		   
				} 	
				failure {
					emailext body: 'Check console SC output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
					to: "${EMAIL_TO}", 
					subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
				}
				unstable {
				    emailext body: 'Check console SC output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
					to: "${EMAIL_TO}", 
					subject: 'Build Unstable in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
				}
				changed {
				    emailext body: 'Check console SC output at $BUILD_URL to view the results.', 
					to: "${EMAIL_TO}", 
					subject: 'Jenkins build is back to normal: $PROJECT_NAME - #$BUILD_NUMBER'
				}
		    }        	
        }
        stage ('Compile Stage Test') {
            steps {                
                    bat 'mvn test'                            	
        	}
		 	post {
				always {
					emailext body: 'Check console ST output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
					to: "${EMAIL_TO}", 
					subject: 'Build begin in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
					
					archive "target/**/*"
            		junit 'target/surefire-reports/*.xml'		   
				} 	
				failure {
					emailext body: 'Check console ST output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
					to: "${EMAIL_TO}", 
					subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
				}
				unstable {
				    emailext body: 'Check console ST output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
					to: "${EMAIL_TO}", 
					subject: 'Build Unstable in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
				}
				changed {
				    emailext body: 'Check console ST output at $BUILD_URL to view the results.', 
					to: "${EMAIL_TO}", 
					subject: 'Jenkins build is back to normal: $PROJECT_NAME - #$BUILD_NUMBER'
				}
		    }        	
        }        
        stage ('Compile Stage Install') {
            steps {         
                    bat 'mvn install'            	
        	}
		 	post {
				always {
					emailext body: 'Check console SI output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
					to: "${EMAIL_TO}", 
					subject: 'Build begin in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'		   
				} 	
				failure {
					emailext body: 'Check console SI output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
					to: "${EMAIL_TO}", 
					subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
				}
				unstable {
				    emailext body: 'Check console SI output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
					to: "${EMAIL_TO}", 
					subject: 'Build Unstable in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
				}
				changed {
				    emailext body: 'Check console SI output at $BUILD_URL to view the results.', 
					to: "${EMAIL_TO}", 
					subject: 'Jenkins build is back to normal: $PROJECT_NAME - #$BUILD_NUMBER'
				}
		    }        	
        }		
    }   
}