pipeline{
  agent any
  stages{
      stage("Scan"){
        steps{
         withSonarQubeEnv('SonarQube') {
         sh "mvn clean package sonar:sonar"
         }
        }
      }
      stage("Build"){
        steps{
            sh '"mvn" -Dmaven.test.failure.ignore clean install'
             }
            }
      stage("deploy"){
       steps{
          deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://13.233.65.211:8080/')], contextPath: null, war: '**/*.war'          
          }
        }
      }
    }
	
	
