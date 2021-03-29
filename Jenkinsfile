pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'D:\\programs\\gradle-5.6-bin\\gradle-5.6\\bin\\gradle.bat build'
        archiveArtifacts(artifacts: 'build/libs/*.jar', allowEmptyArchive: true)
      }
      post {
        failure {
            steps {
                mail(subject: 'Build Project', body: 'The build failed', cc: 'hb_chergui@esi.dz')
            }
        }
      }
    }

    stage('Mail notification') {
      steps {
        mail(subject: 'Build Project', body: 'The build was successful', cc: 'hb_chergui@esi.dz')
      }
    }

  }
}