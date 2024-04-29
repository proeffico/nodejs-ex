pipeline {
	agent {
		label 'nodejs'
	}
	
    	stages {
		
		stage('Build Image') {
			steps {
				echo "Building image..."
				sh "oc apply -f cicd/Build-Config.xml"
				sh "oc start-build buildconfig/nodejs-ex-git"
			}
		}
	}
}
