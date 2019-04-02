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
	}
}

