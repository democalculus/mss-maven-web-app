@Library('jenkins-slack-library') _
//${library.jenkins-slack-library.version}

pipeline {
  agent any
  tools {
      maven 'UI_Maven3..9.9'
  }

  environment {
    deploymentName = "devsecops"
    containerName = "devsecops-container"
    serviceName = "devsecops-svc"
    imageName = "siddharth67/numeric-app:${GIT_COMMIT}"
    applicationURL = "http://devsecops-demo.eastus.cloudapp.azure.com"
    applicationURI = "/increment/99"
  }

  stages {
    stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/walmart-dev-mss']], extensions: [], userRemoteConfigs: [[credentialsId: 'democalculus-github-login-creds', url: 'https://github.com/democalculus/maven-web-application.git']]])
            }
        }

    stage ('Build') {
      steps {
      sh 'mvn  clean install'
       }
    }

  }

  post {
    //    always {
    //      junit 'target/surefire-reports/*.xml'
    //      jacoco execPattern: 'target/jacoco.exec'
    //      pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
    //      dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
    //      publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'owasp-zap-report', reportFiles: 'zap_report.html', reportName: 'OWASP ZAP HTML Report', reportTitles: 'OWASP ZAP HTML Report'])

    // Use sendNotifications.groovy from shared library and provide current build result as parameter
    //      sendNotification currentBuild.result
    //    }

    success {
      script {
        /* Use slackNotifier.groovy from shared library and provide current build result as parameter */
        env.failedStage = "none"
        env.emoji = ":white_check_mark: :tada: :thumbsup_all:"
        sendNotification currentBuild.result
      }
    }

    // failure {

    // }
  }

  }
