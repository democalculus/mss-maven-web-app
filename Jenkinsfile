node {

    def mvnHome = tool name: "UI_Maven3..9.9"
    environment {
        // jenkins_server_url = "http://ec2-3-129-59-179.us-east-2.compute.amazonaws.com:8090"
        notification_channel = 'mss-java-web-app'
        slack_url = 'https://hooks.slack.com/services/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'

       }
    stage ("checkout")  {
       checkout([$class: 'GitSCM', branches: [[name: '*/walmart-dev-mss']], extensions: [], userRemoteConfigs: [[credentialsId: 'democalculus-github-login-creds', url: 'https://github.com/democalculus/maven-web-application.git']]])
    }

   stage ('build')  {
    sh "${mvnHome}/bin/mvn  clean install"
    }

   //   stage ('Code Quality scan')  {
   //     withSonarQubeEnv('sonar_creds') {
   //     sh "${mvnHome}/bin/mvn sonar:sonar"
   //      }
   // }
   //
   // stage ('Code coverage')  {
   //        jacoco()
   //     }
   // stage ('Nexus upload')  {
   //      nexusArtifactUploader(
   //      nexusVersion: 'nexus3',
   //      protocol: 'http',
   //      nexusUrl: '34.125.253.100:8081',
   //      groupId: 'Dev',
   //      version: '1.0-SNAPSHOT',
   //      repository: 'demo-walmart-snapshop',
   //      credentialsId: 'nexus_creds_id',
   //      artifacts: [
   //          [artifactId: 'maven-web-app',
   //           classifier: '',
   //           file: 'maven-web-app.war',
   //           type: 'war']
   //      ]
   //   )
   //  }

   stage ('DEV Deploy')  {
      echo "deploying to DEV Env "
      //deploy adapters: [tomcat9(credentialsId: '4c55fae1-a02d-4b82-ba34-d262176eeb46', path: '', url: 'http://your_tomcat_url:8080')], contextPath: null, war: '**/*.war'
      deploy adapters: [tomcat9(credentialsId: 'apache-tomcat-10-username-passord', path: '', url: 'http://18.118.206.5:8085')], contextPath: 'mss-walmart-dev', war: '**/*.war'

    }
}

  // stage ('Slack notification')  {
  //   slackSend(channel:'channel-name', message: "Job is successful, here is the info -  Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
  //  }

//    stage ('DEV Approve')  {
//             echo "Taking approval from DEV Manager for QA Deployment"
//             timeout(time: 7, unit: 'DAYS') {
//             input message: 'Do you approve QA Deployment?', submitter: 'admin'
//             }
//      }
//
// stage ('QA Deploy')  {
//      echo "deploying into QA Env "
// deploy adapters: [tomcat9(credentialsId: '4c55fae1-a02d-4b82-ba34-d262176eeb46', path: '', url: 'http://your_tomcat_url:8080')], contextPath: null, war: '**/*.war'
//
// }
//
//
//   stage ('QA notify')  {
//     slackSend(channel:'channel-name', message: "QA Deployment was successful, here is the info -  Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
//    }
//
// stage ('QA Approve')  {
//     echo "Taking approval from QA manager"
//     timeout(time: 7, unit: 'DAYS') {
//         input message: 'Do you want to proceed to PROD Deploy?', submitter: 'admin,manager_userid'
//   }
// }
//
// stage ('PROD Deploy')  {
//      echo "deploying into PROD Env "
// deploy adapters: [tomcat9(credentialsId: '4c55fae1-a02d-4b82-ba34-d262176eeb46', path: '', url: 'http://your_tomcat_url:8080')], contextPath: null, war: '**/*.war'
//
 // }
// }
def notifySlack(text, channel, attachments) {

    def payload = JsonOutput.toJson([text: text,
        channel: channel,
        attachments: attachments
    ])

    sh "curl -X POST --data-urlencode \'payload=${payload}\' ${slack_url}"
}
