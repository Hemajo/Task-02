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
				  nexusUrl: '65.1.147.114:8081', 
				  nexusVersion: 'nexus3', 
				  protocol: 'http', 
				  repository: 'Jenkins_Assignment_3', 
				  version: '1.${BUILD_NUMBER}'
		  }
	  }
	  stage('Pull the Artifact from Nexus and Deploy on Production') {
		  agent any
		  steps {
			  sh 'curl -X GET http://65.1.147.114:8081/repository/Jenkins_Assignment_3/com/blazeclan/DevOpsDemo/${BUILD_NUMBER}/DevOpsDemo-${BUILD_NUMBER}.war -o DevOpsDemo-${BUILD_NUMBER}.war'
			  deploy adapters: [
				  tomcat8(credentialsId: 'tomcat', 
					  path: '', 
					  url: 'http://13.233.156.155:8080/')
			  ], 
				  contextPath: 'DevOpsDemo-App',
				  war: '**/*.war'
		  }
	  }
  }
}
 
    
	
	
