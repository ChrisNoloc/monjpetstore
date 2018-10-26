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
    stage('qualimetrie') {
      steps {
        withSonarQubeEnv ('Sonar'){
          bat(script: 'runqualimetrie.bat', encoding: 'UTF-8')
        }
      }
    }
    stage('quality gate') {
      steps {
        sleep 10
        timeout(time: 1, unit: 'MINUTES') {
          waitForQualityGate(abortPipeline: true)
        }
      }
    }
    stage('Publication') {
      steps {
        nexusArtifactUploader(artifacts: [
                    [artifactId: 'jpetstore', type: 'war', classifier: 'debug', file: 'target/jpetstore.war']
                  ], nexusVersion: 'nexus3', protocol: 'http', nexusUrl: 'localhost:8081/', groupId: 'jpetstore', version: '1.0-SNAPSHOT', repository: 'maven-snapshots', credentialsId: 'nexus')
        }
      }
    }
  }