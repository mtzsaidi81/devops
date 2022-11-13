pipeline {
    agent any

    stages {
        
                stage('git') {
            steps {
            
                git branch: 'moetaz', url: 'https://github.com/mtzsaidi81/devops.git',
                credentialsId:"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";
                
            }
}
        
        stage('database connection') {
            steps{
                sh '''
                sudo docker stop mysql || true
                sudo docker restart mysql || true
                '''
            }
        }
        stage('cleanig the project') {
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
        stage ('SonarQube analysis') {
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
	    /*
      stage('Push') {

			steps {
				sh 'docker push moetaz081/achat'
			}
		}
        */
       stage('Run app With DockerCompose') {
              steps {
                  sh "docker-compose -f docker-compose.yml up -d  "
              }
              }
	     
        stage('Sending email'){
           steps {
            mail bcc: '', body: '''Hello from Jenkins,
            Devops Pipeline returned success.
            Best Regards''', cc: '', from: '', replyTo: '', subject: 'Devops Pipeline', to: 'mtzsaidi81@gmail.com'
            }
       }
       

    }
}
