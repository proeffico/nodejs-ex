pipeline {
	agent {
		label 'nodejs'
	}
	environment {
		SONAR_PASSWORD = credentials('Sonar.Admin')
	}
    	stages {
    		
		stage('SonarQube Analysis') {
		    steps {
			withSonarQubeEnv(installationName: 'SonarQube Scanner') {
			    sh """/opt/sonarqube/bin/sonar-scanner \
			    -D sonar.exclusions=vendor/**,storage/** \
			    -D sonar.projectKey=OpenShiftNodeJS \
			    -D sonar.sourceEncoding=UTF-8 \
			    -Dsonar.host.url=http://sonarqube.proeffico.com \
			    -Dsonar.login=$SONAR_PASSWORD"""
			}
		    }
		}

		stage("Quality Gate") {
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

