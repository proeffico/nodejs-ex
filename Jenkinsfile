pipeline {
	agent {
		label 'nodejs'
	}
	environment {
		SONAR_PASSWORD = credentials('Sonar.Admin')
	}
    	stages {
    		stage('SonarQube analysis') {
    			environment {
        			scannerHome = tool 'SonarQube Scanner' // the name you have given the Sonar Scanner (in Global Tool Configuration)
    			}
    			steps {
        			withSonarQubeEnv(installationName: 'SonarQube Scanner') {
            				sh """${scannerHome}/bin/sonar-scanner -X \
    						-Dsonar.projectKey=OpenShiftNodeJS \
    						-Dsonar.sourceEncoding=UTF-8 \
    						-Dsonar.host.url=http://192.168.29.171:9000 \
    						-Dsonar.login=$SONAR_PASSWORD
						"""
			        }
    			}
		}

		stage('Quality Gate') {
		    steps {
		      timeout(time: 1, unit: 'HOURS') {
			waitForQualityGate abortPipeline: true
		      }
		    }
		}
		stage('Install Package') {
			steps {
				sh "npm install"
			}
		}
		stage('Build') {
			steps {
				echo "Building image..."
				sh "npm start"
			}
		}
	}
}

