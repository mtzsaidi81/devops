pipeline {
    agent any

    stages {
        
                stage('Git') {
            steps {
            
                git branch: 'moetaz', url: 'https://github.com/mtzsaidi81/devops.git',
                credentialsId:"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";
                
            }
}
        
       
        }
        stage('Cleaning the project') {
            steps{
                sh 'mvn clean'
            }

        }
        stage ('artifact construction') {
            steps{
                sh 'mvn  package'
            }
        }
        stage ('Unit Test') {
            steps{
                sh 'mvn  test'
            }
        }
        stage ('Code Quality Check via SonarQube') {
            steps{
                sh '''
                mvn sonar:sonar
                '''
            }
        }
	    
        stage('Nexus'){
            steps{
                sh """mvn deploy """
            }
        }
	    
         stage('Docker build')
        {
            steps {
                 sh 'docker build --build-arg IP=localhost -t moetaz081/achat  .'
            }
        }
        stage('Docker login')
        {
            steps {
                sh 'echo $dockerhub_PSW | docker login -u moetaz081 -p dckr_pat_520jPRGk-4XZFRzJxcpnTPF4xE4'
            }    
       
        }
	    
     /* stage('Push') {

			steps {
				sh 'docker push moetaz081/achat'
			}
		}*/ 
	
       stage('Run app With DockerCompose') {
              steps {
                  sh "docker-compose -f docker-compose.yml up -d  "
              }
              }
	     
       
       }
       

    }



}
