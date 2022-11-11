pipeline{
    agent any
    
    stages{
        stage("GIT ") {
            steps {
                git branch: 'main', credentialsId: 'git', url: 'https://github.com/mtzsaidi81/devops.git'
            }
        }
        stage('MVN CLEAN'){
            steps{
                sh "mvn clean"
            }
        }
        stage('MVN COMPILE'){
            steps{
                sh "mvn compile"
            }
        }
        stage('MVN SONARQUBE'){
            steps{
                sh "mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=moetaz"
            }
        }
        /*
        
        stage('MVN TEST'){
            steps{
                sh "mvn test"
            }
        }*/
        stage('MVN BUILD'){
            steps{
                sh "mvn clean install package -Dmaven.test.skip=true"
                
            }
        }
        
        stage('NEXUS'){
            steps{
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'tpAchatProject', 
                        classifier: '', 
                        file: 'target/tpAchatProject-1.0.jar', 
                        type: 'jar'
                    ]
                ], 
                credentialsId: 'nexus3', 
                groupId: 'com.esprit.examen', 
                nexusUrl: '192.168.44.128:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'achat-adam-release', 
                version: '1.0' 
            }
        }
        stage('Build Docker Image') {
                 steps {
                 sh 'docker build -t adamelamri/adamback:1.0.0 .'
                 }
              }

              stage('Push Docker Image') {
                   steps {
                     sh 'docker login -u "adamelamri" -p "5;X#,;+5Z_PfvfM" docker.io'
                     sh 'docker push adamelamri/adamback:1.0.0 '
                   }
              }
        stage('DOCKER COMPOSE'){
            steps{
                //sh 'docker-compose --version'
                //sh  'docker compose ps'
                sh 'docker-compose up'
                
                
            }
        }
        
    }
    post {
                      success {
                        
                            emailext body: 'Pipeline build successfully', subject: 'Pipeline build', to: 'saidi.moetaz@esprit.tn'
                      }
                      failure {
                        
                            emailext body: 'Pipeline failure', subject: 'Pipeline failure', to: 'saidi.moetaz@esprit.tn'
                      }
              }
}
