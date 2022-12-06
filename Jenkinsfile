pipeline {
  agent {
    node {
      label 'Jenkins-Slave'
    }
  }
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
				  credentialsId: 'nexus', 
				  groupId: 'com.blazeclan', 
				  nexusUrl: '3.110.41.148:8081', 
				  nexusVersion: 'nexus3', 
				  protocol: 'http', 
				  repository: 'Jenkins_Assignment_3', 
				  version: '1.${BUILD_NUMBER}'
		  }
	  }
	  stage('Pull the Artifact from Nexus and Deploy on Production') {
		  agent any
		  steps {
			  sh 'curl -X GET http://mohit:highrisk@3.110.218.42:8081/nexus/service/local/repositories/maven-central/content/com/blazeclan/DevOpsDemo/${BUILD_NUMBER}/DevOpsDemo-${BUILD_NUMBER}.war -o DevOpsDemo-${BUILD_NUMBER}.war'
			  deploy adapters: [
				  tomcat8(credentialsId: 'tomcat-cred', 
					  path: '', 
					  url: 'http://13.233.146.83:8080/')
			  ], 
				  contextPath: 'DevOpsDemo-App',
				  war: '**/*.war'
		  }
	  }
  }
}
 
    
	
	
