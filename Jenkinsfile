pipeline {
	agent {
		label 'nodejs'
	}
    	stages {
    		stage('Download Repo') {
		    steps {	script {
				git(url: 'https://interface/.git', branch: 'master', credentialsId: 'MyPwd')
			}}
		}
		stage('SonarQube Analysis') {
		    steps {
			withSonarQubeEnv(installationName: 'SonarQube Scanner') {
			    sh """/opt/sonarqube/bin/sonar-scanner \
			    -D sonar.exclusions=vendor/**,storage/** \
			    -D sonar.projectKey=OpenShiftNodeJS \
			    -D sonar.sourceEncoding=UTF-8 \
			    -Dsonar.host.url=http://sonarqube.proeffico.com \
			    -Dsonar.login=sqa_769eb0ed2dd20f00c4e40f8d7c0bebb1e1b699dc"""
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

