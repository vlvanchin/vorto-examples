node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // clean workspace
      cleanWs()
      // Get some code from a GitHub repository
      git branch: 'fix-build', changelog: false, poll: false, url: 'https://github.com/vlvanchin/vorto-examples.git'

      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.
      mvnHome = tool 'maven_3.3.9'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
        dir('vorto-generators') {
            sh "'${mvnHome}/bin/mvn' clean install"
        }
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Results') {
      junit allowEmptyResults: true, testResults: '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Changelogs') {
       def changelogString = gitChangelog from: [type: 'REF', value: 'master'], returnType: 'STRING', template: '''<h1> Git Changelog changelog </h1>

<p>
Changelog of Git Changelog.
</p>

{{#tags}}
<h2> {{name}} </h2>
 {{#issues}}
 {{#hasIssue}}
 {{#hasLink}}
<h2> {{name}} <a href="{{link}}">{{issue}}</a> {{title}} </h2>
 {{/hasLink}}
 {{^hasLink}}
<h2> {{name}} {{issue}} {{title}} </h2>
 {{/hasLink}}
 {{/hasIssue}}
 {{^hasIssue}}
<h2> {{name}} </h2>
 {{/hasIssue}}


 {{#commits}}
<a href="https://github.com/vlvanchin/vorto-examples/commit/{{hash}}">{{hash}}</a> {{authorName}} <i>{{commitTime}}</i>
<p>
<h3>{{{messageTitle}}}</h3>

{{#messageBodyItems}}
 <li> {{.}}</li>
{{/messageBodyItems}}
</p>


 {{/commits}}

 {{/issues}}
{{/tags}}''', to: [type: 'REF', value: 'fix-build']
      currentBuild.description = changelogString
   }
   stage('UploadToS3') {
      withAWS(region:'eu-central-1',credentials:'aws-s3-vorto-jenkins-technical-user') {
          s3Upload( bucket: 'pr-vorto-documents',  file: 'vorto-generators/generator-runner/target/generator-runner-3rd-party-exec.jar', path: 'example-generators/${CHANGE_ID}/${BUILD_NUMBER}/generator-runner-3rd-party-exec.jar')
      }
   }
}
