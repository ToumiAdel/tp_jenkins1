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
                bat 'gradlew publish'

            }
           
        }
  }


}
