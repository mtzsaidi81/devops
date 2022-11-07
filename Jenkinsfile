pipeline {
  agent any
 
  stages {
      stage("GIT CHECKOUT") {
            steps {
                git branch: 'main', credentialsId: 'git', url: 'https://github.com/mtzsaidi81/devops.git'
            }
        }
        stage('MVN CLEAN') {
      steps {
        sh 'mvn clean '
      }
    }
    stage('MVN COMPILE') {
      steps {
            sh 'mvn compile '
      }
    }
    
       stage('MVN TEST') {
            steps {
                sh 'mvn test'
           }
        }
       stage("Maven BUILD") {
            steps {
                script {
                    sh "mvn install -DskipTests=true"
                }
            }
        }

        stage ('Sonar') {
            steps {
               withSonarQubeEnv(installationName: 'jenkins-sonar', credentialsId: 'jenkins-sonar') {
                 sh 'mvn clean package sonar:sonar'
                }
            }
        
          
        }
stage('MVN DEPLOY') {
      steps {
            sh 'mvn deploy -DskipTests=true '
      }
    }
  






}



}
