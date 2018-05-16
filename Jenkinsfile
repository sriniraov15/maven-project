pipeline{
	agent any

	tools {
        maven 'localMaven'
    }

    parameters {

    	string(name: 'tomcat_stage', defaultValue: '52.14.153.52', description: 'Staging Server')
    	string(name: 'tomcat_production', defaultValue: '18.188.99.51', description: 'Production Server')

    }

    triggers {
    	pollSCM('* * * * *')
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

		stage ('Deployments'){
			parallel {
				stage ('Deploy to Staging') {
					steps {
						bat "winscp -i \\AWSKeys\\tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_stage}:\\var\\lib\\tomcat7\\webapps"
					}
					post {
						success {
							echo 'Code deployed to Staging'
						}
						failure {
							echo 'Deploy to STAGE failed...'
						}
					}
				}

				stage ('Deploy to Production') {
					steps {
						bat "winscp -i /AWSKeys/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_production}:/var/lib/tomcat7/webapps"
					}
					post {
						success {
							echo 'Code deployed to Production'
						}
						failure {
							echo 'Production Deployment failed...'
						}
					}
				}
			}
		}
	}
}