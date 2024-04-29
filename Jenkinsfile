pipeline {
	agent {
		label 'nodejs'
	}
	
		stage('Build Image') {
			steps {
				echo "Building image..."
				sh "oc apply -f cicd/Build-Config.xml"
				sh "oc start-build buildconfig/${APP_NAME} --from-file=target/${APP_JAR}"
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

