pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage("sonar quality check"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'chmod +x gradlew' //give permission to gradlew file
                            sh './gradlew sonarqube'
                    }

                    timeout(time: 1, unit: 'HOURS') {
                                          def qg = waitForQualityGate()
                                          if (qg.status != 'OK') {
                                               error "Pipeline aborted due to quality gate failure: ${qg.status}" // test rules
                                          }
                    }

                }
            }
        }
    }
}
