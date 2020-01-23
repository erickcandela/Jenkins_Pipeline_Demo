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

        NEXUS_URL = "127.0.0.1:8081"

        // Repository where we will upload the artifact

        NEXUS_REPOSITORY = "NEXUS_CONFIG"

        // Jenkins credential id to authenticate to Nexus OSS

        NEXUS_CREDENTIAL_ID = "7e3feb4a-d9aa-40e1-8008-4cbf81e6b20b"       
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
		stage('Compile Stage SonarQube') {
			steps { 
				withSonarQubeEnv('SERVER_SONARQUBE') {
	      			sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
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
                    echo "${filesByGlob[0].name}; ${filesByGlob[0].path}; ${filesByGlob[0].directory}; ${filesByGlob[0].length}; ${filesByGlob[0].lastModified}"

                    // Extract the path from the File found
                    artifactPath = filesByGlob[0].path;					
					echo "artifactPath: ${artifactPath}";
					
					// Assign to a boolean response verifying If the artifact name exists
                    artifactExists = fileExists artifactPath;					
					echo "artifactExists: ${artifactExists}";
					
                    if(artifactExists) {
                        echo "*** File Begin: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader (
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                // Lets upload the pom.xml file for additional information for Transitive dependencies
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );                        
                        echo "*** File End";
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }

                }
            }
        }	
    }   
}