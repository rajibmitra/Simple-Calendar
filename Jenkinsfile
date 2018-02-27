pipeline {

  agent none
  stages {
    stage('Checkout Code'){
      steps {
        node('master') {
          deleteDir()
          checkout scm
          stash 'code'
        }
      }
    }
	stage('Starting Android Emulator') {
      steps {
        parallel(
          'Android Emulator':{
            node('master') {
              deleteDir()
	      unstash 'code'
	      sh 'echo "running android emulator"'
		
            }
          }
        )
      }
    }
    stage('Build Application') {
      steps {
        parallel(
          'Build Application':{
            node('master') {
	       //withMaven(maven: 'mvn3.2.5') {
		     deleteDir()
	     	     unstash 'code'
              	     sh 'echo "building app"'
		    // archiveArtifacts 'target/*.war'
		    // stash name:'war_file', includes: '*.war'
		}
		
            }
          
        )
      }
    }
    stage('APK Sign') {
      steps {
        parallel(
          'create & deploy':{
            node('master') {
              deleteDir()
	      unstash 'code'
              sh 'echo "HELLO"'
	      sh '"signing apk"'
            }
          }
        )
      }
    }

	stage('Testing') {
      steps {
        parallel(
          'Testing':{
            node('master') {
              deleteDir()
	      build 'integration_testing'
              sh 'echo "testting"'
            }
          }
        )
      }
    }

  }

   }
