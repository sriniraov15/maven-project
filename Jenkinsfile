pipeline{
	agent any

	tools {
        maven 'localMaven'
    }

	stages{
		stage('Build'){
			steps {
				echo 'Executing bat...'
				bat 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving.....'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
		stage ('Deploy to Staging'){
			steps {
				build job: 'deploy-to-staging'
			}
			post {
				success {
					echo 'Deploy to Staging is success'
				}
			}
		}
	}
}