pipeline {
   /** agent { 
        docker {
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
       }
    } **/
    agent any
    tools {
      maven 'maven' 
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
          stage('SonarQube Analysis') {
           steps {
             script {
             withSonarQubeEnv('sonarqube') {
               sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=saadalsayed_simple-java-maven-app_AYkmREvWrdsVyKIfIFEG -Dsonar.projectName='simple-java-maven-app'"
             }
            }
           }
          }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
