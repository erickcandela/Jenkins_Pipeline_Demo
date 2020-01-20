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
        stage('Compile Stage Publish') {
     		nexusPublisher nexusInstanceId: 'localNexus', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'war/target/jenkins-pipeline-demo.war']], mavenCoordinate: [artifactId: 'jenkins-pipeline-demo-war', groupId: 'org.jenkins-ci.main', packaging: 'war', version: '1.1']]]
       }
    }   
}