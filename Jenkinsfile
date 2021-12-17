node{
	
	def mavenHome = tool name: "maven 3.8.4"
	stage('CheckOutCode')
	{
		git branch: 'development', credentialsId: '87f4b58b-321e-40d7-a4e2-c43b1eb305d5', url: 'https://github.com/pnr-do-project-august/maven-web-application.git'
	}
	stage('Build')
	{
		sh "${mavenHome}/bin/mvn clean package"
	}
	stage('SonarQubereport')
	{
		sh "${mavenHome}/bin/mvn clean sonar:sonar"
	}
	stage('UploadArtifactIntoNexus')
	{
		sh "${mavenHome}/bin/mvn clean deploy"
	}
	stage('DeployAppIntoTomcatServer')
	{
		sshagent(['93ec5659-405d-4a36-ab73-78b840699073']) {
			sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.68.39:/opt/tomcat9/webapps/"
		}
	}
	stage('EmailNotification')
	{
	    emailext body: '''Cograts to said . The build is over

        Regards,
        Pulikanti Nageswarreddy''', subject: 'Build is over', to: 'nageswarreddy447@gmail.com'
	}
}
