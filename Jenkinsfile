
pipeline
{
	agent any
	tools{
	maven "Maven_3.9.9"
	}
	
	stages
	{
		stage('code checkout')
		{
			steps{
					git branch: 'qa', url: 'https://github.com/newton9979/maven-webapplication-project-kkfunda.git'
				 }
		}
		
		stage('Build')
		{
			steps{
					sh  "mvn clean package"
				}
		}
		
		stage('SQ-Report')
		{
				steps{
					sh "mvn sonar:sonar"
				}
		}
		
		stage('upload to Nexus')
		{
			steps{
				sh "mvn deploy"
				}
		}
		
		stage('deploy to tomcat')
		{
			steps{
				deploy adapters: [
					tomcat9(
							credentialsId: 'tomcat',
							url: 'http://13.126.65.220:8080'
						)
				],
				contextPath:'/maven-web-application', // target path 
				war: 'target/*.war' // source path 
				
				}
		}
		
	} //stages end

}//pipeline close
