//@Library('jenkins-slack-library') _
//${library.jenkins-slack-library.version}

pipeline {
  agent any
  tools {
      maven 'UI_Maven3..9.9'
  }

  environment {
    containerName = "http://3.143.147.204:8085/mss-walmart-qa-app/"
    serviceName = "devsecops-svc"
    imageName = "siddharth67/numeric-app:${GIT_COMMIT}"
    applicationURL = "http://3.143.147.204:8085/mss-walmart-dev"
    deploymentName = "http://3.143.147.204:8085/mss-walmart-dev-app/"
    //containerName = "devsecops-container"
    //serviceName = "devsecops-svc"
  //  imageName = "siddharth67/numeric-app:${GIT_COMMIT}"
     jenkins_server_url = "http://3.143.147.204:8080/"
    mss_web_app = "/increment/99"
  }

  stages {
    stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/mss-walmart-dev']], extensions: [], userRemoteConfigs: [[credentialsId: 'democalculus-github-login-creds', url: 'https://github.com/democalculus/mss-maven-web-app.git']]])
            }
        }

    stage ('Build') {
      steps {
      sh 'mvn  clean install'
       }
    }

    stage ('DEV Deploy') {
      steps {
      echo "deploying to DEV Env "
      deploy adapters: [tomcat9(credentialsId: 'apache-tomcat-9-username-passord', path: '', url: 'http://3.143.147.204:8085/')], contextPath: 'mss-walmart-dev-app', war: '**/*.war'
      }
    }

    stage('QA approve') {
            steps {
                input('Do you want to proceed?')
            }
        }
    //
    // stage('QA approve') {
    //     steps {
    //       notifySlack("Do you approve QA deployment? $jenkins_server_url/job/$JOB_NAME", notification_channel, [])
    //         input 'Do you approve QA deployment?'
    //         }
    //     }

    stage ('QA Deploy') {
      steps {
      echo "deploying to QA Env "
      deploy adapters: [tomcat9(credentialsId: 'apache-tomcat-9-username-passord', path: '', url: 'http://3.143.147.204:8085/')], contextPath: 'mss-walmart-qa-app', war: '**/*.war'
      }
    }

    stage ('Deploy kops cluster') {
      steps {
          sh "ls -lart"
      sshagent(['kops-cluster-private-key-login']) {
           sh "kubectl apply -f maven-web-app.yml -n jenkins-ns "
          }
      }
    }

  }

  post {
       always {
    //      junit 'target/surefire-reports/*.xml'
    //      jacoco execPattern: 'target/jacoco.exec'
    //      pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
    //      dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
    //      publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'owasp-zap-report', reportFiles: 'zap_report.html', reportName: 'OWASP ZAP HTML Report', reportTitles: 'OWASP ZAP HTML Report'])

    // Use sendNotifications.groovy from shared library and provide current build result as parameter
         sendNotification currentBuild.result
       }
     //this is my note for success alert and this line will be deleted
    // success {
    //   script {
    //     /* Use slackNotifier.groovy from shared library and provide current build result as parameter */
    //     env.failedStage = "none"
    //     env.emoji = ":white_check_mark: :tada: :thumbsup_all:"
    //     sendNotification currentBuild.result
    //   }
    // }

    // failure {

    // }
  }

  }
