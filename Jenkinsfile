pipeline{
  agent any
  stages{
	stage("Build"){
		steps{
			sh '"mvn" -Dmaven.test.failure.ignore clean install'
		}
            }
      stage("Scan"){
        steps{
         withSonarQubeEnv('SonarQube') {
         sh "mvn clean package sonar:sonar"
         }
        }
      }
      stage("deploy"){
       steps{
          deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://13.233.65.211:8080/')], contextPath: null, war: '**/*.war'          
          }
        }
	  stage("Upload to Nexus"){
		  steps{
			  nexusArtifactUploader artifacts: [[artifactId: 'DevOpsDemo', classifier: '', file: '/target/DevOpsDemo.war', type: 'war']], credentialsId: '26c5a723-9235-4c81-bc18-4bebb5b5a57f', groupId: 'com.blazeclan', nexusUrl: '15.206.172.101:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'nexus_pipeline', version: '1.0-SNAPSHOT'
		  }
	  }
  }
}
    
	
	
