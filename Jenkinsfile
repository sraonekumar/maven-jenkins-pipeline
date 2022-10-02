pipeline {
  agent any
  tools {
        maven "Maven 3.8.6" 
   }

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }  
       }
      stage('Test Maven - JUnit') {
            steps {
              sh "mvn test"
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
              }
            }
        }
        

      stage('Sonarqube Analysis - SAST') {
            steps {
                  withSonarQubeEnv('SonarQube') {
           sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=sravan-maven-project \
  -Dsonar.host.url=http://23.100.43.124:9000 \
  -Dsonar.login=sqp_133fe78ffb46fa42c09df873b07ef8ec223d7302" 
                }
           timeout(time: 2, unit: 'MINUTES') {
                      script {
                        waitForQualityGate abortPipeline: true
                    }
                }
              }
        }
     }
}
