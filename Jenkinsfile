pipeline {
  agent any
  stages {
    stage('Récupération des sources') {
      steps {
        git(url: 'https://github.com/ChrisNoloc/monjpetstore.git', credentialsId: 'logingithub', branch: 'master')
      }
    }
    stage('Build Maven') {
      steps {
        bat(script: 'runmaven.bat', encoding: 'UTF-8')
      }
    }
  }
}