pipeline {
    agent any
	stages {
		stage('UploadToS3'){
			steps{
				withAWS(region:'eu-central-1',credentials:'aws-s3-vorto-jenkins-technical-user') {
					sh 'aws s3 ls --profile aws-s3-vorto-jenkins-technical-user --no-verify-ssl'
				}
			}
		}		
	}
}

