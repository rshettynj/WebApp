//Deploy_To_Nexus_Pipeline
node('master'){
	stage('Checkout'){
		//Checkout the code from a GitHub repository
		git credentialsId: 'jenkinsGitHub', url: 'https://github.com/anooptcs/WebApp.git'
		}
		
	stage('Build'){
			def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
			def mvnCMD = "${mvnHome}/bin/mvn"
			sh "${mvnCMD} clean compile package"
			archiveArtifacts '**/*.war'
		}
		
	stage('Static Analysis'){
			def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
			def mvnCMD = "${mvnHome}/bin/mvn"
			sh "${mvnCMD} clean test checkstyle:checkstyle"
		}
		
	stage('Analysis Report'){
			checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
		}
		
	stage('SonarQB Analysis'){
			def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
			def mvnCMD = "${mvnHome}/bin/mvn"
			sh "${mvnCMD} clean verify sonar:sonar"
		}
		
	stage ('Deploye to stageing'){
		
	        //copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, projectName: 'Pipeline_As_Code_Build_Test_Deploy', selector: lastSuccessful()
			build job: 'Deploy-to-staging'
		}
		
	stage('Deploy to Nexus'){
			timeout(time:5, unit:'DAYS'){
			input message: 'Approve Nexus Deployment?'
		}
			def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
			def mvnCMD = "${mvnHome}/bin/mvn"
			sh "${mvnCMD} clean deploy"                   
		}
		
	stage ('Deploye to prod'){
			timeout(time:5, unit:'DAYS'){
			input message: 'Approve PROD Deployment?'
		}
			//copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, projectName: 'Pipeline_As_Code_Build_Test_Deploy', selector: lastSuccessful()
			build job: 'Deploy-to-Prod'
		}

	
}
