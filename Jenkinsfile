pipeline {
    agent any
	stages {
		stage('Preparation') {
			steps {
				// clean workspace
				cleanWs()
				// Get some code from a GitHub repository
				git branch: 'fix-build', changelog: false, poll: false, url: 'https://github.com/vlvanchin/vorto-examples.git'
		   }
		}
		stage('Build') {
			steps{
				dir('vorto-generators') {
					withMaven(	maven: 'maven_3.3.9') {
						sh 'mvn -P coverage clean install'
					}
				}
			}
		}
		stage('Results') {
		    steps{
				junit allowEmptyResults: true, testResults: '**/target/surefire-reports/TEST-*.xml'
				archive 'target/*.jar'
			}
		}
		stage('UploadToS3'){
			steps{
				withAWS(region:'eu-central-1',credentials:'aws-s3-vorto-jenkins-technical-user') {
					s3Upload (file: 'generator-runner/target/generator-runner-3rd-party-exec.jar', bucket: 'pr-vorto-documents',  path: 'generator-runner-3rd-party-exec.jar')
				}
			}
		}
	}
}
