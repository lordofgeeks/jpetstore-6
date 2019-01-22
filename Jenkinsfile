pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Hello'
        sh 'mvn clean install -Dlicense.skip=true'
      }
    }
  }
  tools {
    maven 'maven'
  }
}