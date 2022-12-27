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
				  nexusUrl: '65.1.130.9:8081', 
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
			  sh 'wget --user=admin --password=admin http://65.1.130.9:8081/repository/test_env/com/blazeclan/DevOpsDemo/1/DevOpsDemo-1.war'
			  deploy adapters: [
				  tomcat8(credentialsId: 'tomcat', 
					  path: '', 
					  url: 'http://65.2.5.138:8080/')
			  ], 
				  contextPath: '',
				  war: '**/*.war'
		  }
	  }
  }
 post {
    always{
      slackSend channel: '#jenkins', color: 'good', message: 'Build is in progress', teamDomain: 'jenkins', tokenCredentialId: 'slack-test'
	deleteDir()
      }
   }
}
 
    
	
	
