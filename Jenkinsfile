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
    						-Dsonar.projectKey=OpenShiftNodeJS2 \
    						-Dsonar.sourceEncoding=UTF-8 \
    						-Dsonar.host.url=http://192.168.29.171:9000 \
    						-Dsonar.token=$SONAR_PASSWORD"""
			        }
    			}
		}

		stage('Quality gate') {
		    steps {
		      timeout(time: 1, unit: 'HOURS') {
			waitForQualityGate abortPipeline: true
		      }
		    }
		}
		
		stage('Build Image') {
			steps {
				echo "Building image..."
				sh "oc apply -f cicd/Build-Config.xml"
				sh "oc start-build buildconfig/nodejs-ex"
			}
		}
		stage('Deploy Service') {
			steps {
				echo "Deploying service..."
				sh "oc apply -f cicd/Deployment.xml"
			}
		}

	}
}
