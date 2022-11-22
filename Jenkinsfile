pipeline{
  agent any
  stages{
	stage(" Maven Build"){
		steps{
			sh 'mvn clean package'
		}
            }
        stage("Quality Check"){
		steps{
			withSonarQubeEnv('SonarQube') {
				sh "mvn sonar:sonar"
			}
		}
      }
	stage("Upload to Nexus"){
		  steps{
			  nexusArtifactUploader artifacts: [
				  [
					  artifactId: 'DevOpsDemo', 
				   	  classifier: '', 
				          file: 'target/DevOpsDemo.war', 
				          type: 'war'
				  ]
			  ],
				  credentialsId: '26c5a723-9235-4c81-bc18-4bebb5b5a57f', 
				  groupId: 'com.blazeclan', 
				  nexusUrl: 'http://35.154.240.48/:8081', 
				  nexusVersion: 'nexus3', 
				  protocol: 'http', 
				  repository: 'nexus_pipeline', 
				  version: '0.0.1-SNAPSHOT'
		  }
	  }
  }
}
    
	
	
