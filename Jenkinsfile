pipeline {
    agent any
	tools {
        maven 'MAVEN_CONF' 
    }   
     
    
    environment {
        EMAIL_TO = 'erick.candela@gmail.com'
        
        // This can be nexus3 or nexus2

        NEXUS_VERSION = "nexus3"

        // This can be http or https

        NEXUS_PROTOCOL = "http"

        // Where your Nexus is running

        NEXUS_URL = "localhost:8081"

        // Repository where we will upload the artifact

        NEXUS_REPOSITORY = "repository-example"

        // Jenkins credential id to authenticate to Nexus OSS

        NEXUS_CREDENTIAL_ID = "nexus-credentials"  
        
        TMP_PATH_RULEAPPSVER="${env.WORKSPACE}/RULEAPPSVER"      
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
		stage('Results') {
			steps {    
				junit '**/target/surefire-reports/TEST-*.xml'
				archive 'target/*.jar'
			} 
		} 
        stage("Compile Stage Publish") {
            steps {

				script {
                    // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps

                    pom = readMavenPom file: "pom.xml";

                    // Find built artifact under target folder

                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");

                    // Print some info from the artifact found

                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"

                    // Extract the path from the File found

                    artifactPath = filesByGlob[0].path;

					echo "${filesByGlob[0].name}"
					
					nexusPublishernexusInstanceId: 'localNexus', 
					nexusRepositoryId: 'NEXUS_CONFIG', 
					packages: [[$class: 'MavenPackage', 
					mavenAssetList: [[classifier: "123", 
					extension: 'jar', 
					filePath: artifactPath ]], 
					mavenCoordinate: [artifactId: XXX, 
					groupId: 'onp.gob.odm.ruleapp', 
					packaging: 'jar', 
					version: "1.0"]]]
				}
            }
        }
    }   
}