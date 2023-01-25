pipeline {
  agent any 
  stages {
    stage('Test') 
    { 
      
    steps{ 
    bat 'gradlew test'  
   archiveArtifacts 'build/test-results/'
                cucumber reportTitle: 'Cucumber report', 
                fileIncludePattern: 'target/report.json',
                trendsLimit: 10,
                classifications: [ 
                    [  
                       'key': 'Browser',
                        'value': 'Firefox'
                    ]
                ]
                      junit 'build/test-results/test/TEST-Matrix.xml'      
    }
    }
    
       stage ('Code Analysis') { // la phase build
            steps {
                           withSonarQubeEnv('sonar'){
                bat 'gradlew sonarqube'
                            }
            }
         }
    
         stage("Quality gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true}
            }
        }
    
      
                 stage("Build") {
            steps {
                bat 'gradlew build'
                bat 'gradlew javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/'
            }
        }

         stage("deploy") {
            steps {
                bat 'gradle publish'

            }
           
        }
      stage("Notification") {
      steps {
          notifyEvents message: 'Good morning <b>Adel</b>', token: 'ygz6d0l8M7LrxEDlZlGBF8oPKpgJpcN1'
        mail bcc: '', body: '''good morning Adel !!''', cc: '', from: '', replyTo: '', subject: 'mail', to: 'ja_toumi@esi.dz'
      }
    }
    
  }
   post {
        failure {
            mail bcc: '', body: '''Error !!''', cc: '', from: '', replyTo: '', subject: 'Pipleline failiure', to: 'ja_toumi@esi.dz'
        }
  }
  

}

