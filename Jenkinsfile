node('slave1'){
	stage('Checkout'){
		//Checkout the code from a GitHub repository
		git credentialsId: 'jenkinsGitHub', url: 'https://github.com/anooptcs/WebApp.git'
	}
	stage('build'){
		def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean compile"
	}
    stage('test'){
		def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean test checkstyle:checkstyle"
 	}
	stage('test Report'){
        checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
	}
	stage('SonarQb'){
		def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean verify sonar:sonar"
	}
    stage('deploy-to-nexus'){
		def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean deploy"                   
	}
	stage ('Deploye to stageing'){
			steps{
				build job: 'Deploy-to-staging'
			}
		}
	
		stage ('Deploye to prod'){
				steps{
					timeout(time:5, unit:'DAYS'){
						input message: 'Approve PROD Deployment?'
					}
				
					build job: 'Deploy-to-Prod'
				}
			post {
				success {
					echo 'Code Deployed to Prod'
				}
				failure {
					echo 'Deployment Failed'
					}
				}
			}
}