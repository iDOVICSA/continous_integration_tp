pipeline {
  agent any
  stages {
    stage('Build') {
      post {
        failure {
          steps() {
            mail(subject: 'Build Project', body: 'The build failed', cc: 'hb_chergui@esi.dz')
          }

        }

      }
      steps {
        bat 'D:\\programs\\gradle-5.6-bin\\gradle-5.6\\bin\\gradle.bat build'
        bat 'D:\\programs\\gradle-5.6-bin\\gradle-5.6\\bin\\gradle.bat javadoc'
        archiveArtifacts(artifacts: 'build/libs/*.jar', allowEmptyArchive: true)
        archiveArtifacts 'build/reports/tests/test/**/*.*'
      }
    }

    stage('Mail notification') {
      steps {
        mail(subject: 'Build Project', body: 'The build was successful', cc: 'hb_chergui@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('Sonar') {
              bat 'D:\\\\programs\\\\gradle-5.6-bin\\\\gradle-5.6\\\\bin\\\\gradle.bat sonarqube'
            }

          }
        }

        stage('Test Reporting') {
          steps {
            cucumber 'reports/example-report.json'
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        bat 'D:\\programs\\gradle-5.6-bin\\gradle-5.6\\bin\\gradle.bat publish'
      }
    }

    stage('Slack notification') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', channel: 'tech', message: 'this message was sent from jenkins', token: 'T01N20G4C00/B01SJEEGQRL/lB6Zsyq95Envb6wRI09Ny2vu')
      }
    }

  }
}