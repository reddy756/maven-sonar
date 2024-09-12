// Powered by Infostretch 

timestamps {

node () {

	stage ('Maven - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '3d7f7ad1-1513-4141-96cb-1b2ea95ce61c', url: 'https://github.com/reddy756/maven-sonar.git']]]) 
	}
	stage('Static Code Analysis') {
                // Execute static code analysis using SonarQube scanner
                withSonarQubeEnv('SonarQube Server') {
                    bat 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=1234'
                
            }
        }

        stage('Quality Gate') {
                //timeout(time: 10, unit: 'MINUTES'){
                    def qualityGate = waitForQualityGate()
                    if (qualityGate.status != 'OK') {
                        error "Quality Gate failed: ${qualityGate.status}"
		    }
               // }
        }

	stage ('Maven-freestyle - Build') {
 			// Maven build step
	withMaven(maven: 'Maven') { 
 			if(isUnix()) {
 				sh "mvn install tomcat7:deploy " 
			} else { 
 				bat "mvn install tomcat7:deploy " 
			} 
 		} 
	}
}
}
