pipeline{
	agent any
	stages{
		stage('Build'){
			steps {
				echo 'Executing sh...'
			}
			post {
				success {
					echo 'Now Archiving.....'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
	}
}