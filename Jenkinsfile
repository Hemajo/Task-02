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
				  repository: 'test_env', 
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
			  sh 'wget --user=admin --password=admin http://65.1.110.124:8081/repository/test_env/com/blazeclan/DevOpsDemo/3/DevOpsDemo-3.war'
			  deploy adapters: [
				  tomcat8(credentialsId: 'tomcat', 
					  path: '', 
					  url: 'http://13.235.67.41:8080/')
			  ], 
				  contextPath: '',
				  war: '**/*.war'
		  }
	  }
  }
}
 
    
	
	
