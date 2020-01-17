pipeline {
    agent any
	tools {
        maven 'MAVEN_CONF' 
    }   
     
    stages {
        stage ('Compile Stage Clean') {
            steps {                
                    bat 'mvn clean'            	
        	}
        }
        stage ('Compile Stage Test') {
            steps {                
                    bat 'mvn test'            	
        	}
        }        
        stage ('Compile Stage Install') {
            steps {         
                    bat 'mvn install'            	
        	}
        }		
    }
    
    environment {
        EMAIL_TO = 'efalvarezp@indracompany.com'
    }
    
 	post {
		always {
			emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
			to: "${EMAIL_TO}", 
			subject: 'Build begin in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'		   
		} 	
		failure {
			emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
			to: "${EMAIL_TO}", 
			subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
		}
		unstable {
		    emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
			to: "${EMAIL_TO}", 
			subject: 'Unstable build in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
		}
		changed {
		    emailext body: 'Check console output at $BUILD_URL to view the results.', 
			to: "${EMAIL_TO}", 
			subject: 'Jenkins build is back to normal: $PROJECT_NAME - #$BUILD_NUMBER'
		}
    }   
}