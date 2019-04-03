pipeline {
    agent any
	stages {
		stage('UploadToS3'){
			steps{
				withAWS(region:'eu-central-1',credentials:'aws-s3-vorto-jenkins-technical-user') {
					sh '/var/lib/jenkins/.local/bin/aws s3 ls --profile aws-s3-vorto-jenkins-technical-user --no-verify-ssl'
				}
			}
		}		
	}
}

