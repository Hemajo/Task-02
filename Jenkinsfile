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
				  nexusUrl: '65.1.110.124:8081', 
				  nexusVersion: 'nexus3', 
				  protocol: 'http', 
				  repository: 'Jenkins_Assignment_3', 
				  version: '${BUILD_NUMBER}'
		  }
	  }
	  stage('Pull the Artifact from Nexus and Deploy on Production') {
		  agent{
			  node{
				  label 'Jenkins-Slave'
			  }
		  }
		  steps {
			  sh 'wget --user=admin --password=admin http://65.1.110.124:8081/repository/Jenkins_Assignment_3/com/blazeclan/DevOpsDemo/1.2/DevOpsDemo-1.2.war/${BUILD_NUMBER}/DevOpsDemo-${BUILD_NUMBER}.war -o DevOpsDemo-${BUILD_NUMBER}.war'
			  deploy adapters: [
				  tomcat8(credentialsId: 'tomcat', 
					  path: '', 
					  url: 'http://13.235.67.41:8080/')
			  ], 
				  contextPath: 'DevOpsDemo-App',
				  war: '**/*.war'
		  }
	  }
  }
}
 
    
	
	
