pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Hello'
        sh 'mvn clean install -Dlicense.skip=true'
      }
    }
    stage('Testing') {
      parallel {
        stage('Sonar Test') {
          steps {
            sh 'mvn sonar:sonar -Dsonar.host.url=http://13.229.60.59:8081 -Dlicense.skip=true'
          }
        }
        stage('Print tester credentials') {
          steps {
            echo "The tester is ${TESTER}"
          }
        }
        stage('Print build number') {
          steps {
            echo "This is build number ${BUILD_ID}"
          }
        }
      }
    }
    stage('Jfrog push') {
      steps {
        echo 'Pushing?'
        script {
          def server = Artifactory.server "artifactory"
          def buildInfo = Artifactory.newBuildInfo()
          def rtMaven = Artifactory.newMavenBuild()

          rtMaven.tool = 'maven'
          rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'

          buildInfo = rtMaven.run pom: 'pom.xml', goals: "clean install -Dlicense.skip=true"
          buildInfo.env.capture = true
          buildInfo.name = 'jpetstore-6'
          server.publishBuildInfo buildInfo
        }

      }
    }
    stage('deploy prompt') {
      steps {
        input 'Deploy to production'
      }
    }
    stage('Deployment') {
      steps {
        echo 'Successfully deployed'
      }
    }
  }
  tools {
    maven 'maven'
  }
  environment {
    TESTER = 'honey_badger'
  }
}