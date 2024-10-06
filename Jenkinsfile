mport groovy.json.JsonOutput

pipeline {
    agent any
    tools {
        mvnHome  'UI_Maven3..9.9'
    }
    environment {
        jenkins_server_url = "http://18.118.206.5:8080"
        notification_channel = 'mss-java-web-app'
        slack_url = slack_url = 'https://hooks.slack.com/services/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'

    }

   stages {

    stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/walmart-dev-mss']], extensions: [], userRemoteConfigs: [[credentialsId: 'democalculus-github-login-creds', url: 'https://github.com/democalculus/maven-web-application.git']]])
            }
        }
    stage ('Build') {
      steps {
      sh '${mvnHome}/bin/mvn  clean install'

    }

    stage ('DEV Deploy') {
      steps {
      echo "deploying to DEV Env "
      deploy adapters: [tomcat9(credentialsId: 'apache-tomcat-10-username-passord', path: '', url: 'http://18.118.206.5:8085')], contextPath: 'mss-walmart-dev', war: '**/*.war'
      }
    }
    stage('QA approve') {
        steps {
          notifySlack("Do you approve QA deployment? $jenkins_server_url/job/$JOB_NAME", notification_channel, [])
            input 'Do you approve QA deployment?'
            }
        }

    stage ('QA Deploy') {
      steps {
      echo "deploying to QA Env "
      deploy adapters: [tomcat9(credentialsId: 'apache-tomcat-10-username-passord', path: '', url: 'http://18.118.206.5:8085')], contextPath: 'mss-walmart-dev', war: '**/*.war'
      }
    }

    }
}
//
// def notifySlack(text, channel, attachments) {
//
//     def payload = JsonOutput.toJson([text: text,
//         channel: channel,
//         attachments: attach
//     ])
//
//     sh "curl -X POST --data-urlencode \'payload=${payload}\' ${slack_url}"
// }
