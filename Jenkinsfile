// Powered by Infostretch 

timestamps {

node () {

	stage ('Maven - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '900caf5f-a681-49c4-ad6b-a026f64612bf', url: 'https://github.com/ganesh8338/maven-war.git']]]) 
	}
	stage('Static Code Analysis') {
                // Execute static code analysis using SonarQube scanner
                withSonarQubeEnv('SonarQube Server') {
                    bat 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=admin@123'
                
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
