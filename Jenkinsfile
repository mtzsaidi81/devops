pipeline {
    agent any

    stages {
        
                stage('git') {
            steps {
            
                git branch: 'Ghaith', url: 'https://github.com/GhaithBh/Devops.git',
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
                 sh 'docker build --build-arg IP=0.0.0.0 -t ghaithbhs/devops  .'
            }
        }
        stage('Docker login')
        {
            steps {
                sh 'echo $dockerhub_PSW | docker login -u ghaithbhs -p dckr_pat_PvFfLE0rm--tKJiRL1igKeLc2fQ'
            }    
       
        }
      stage('Push') {

			steps {
				sh 'docker push ghaithbhs/devops'
			}
		}
        
       stage('Run app With DockerCompose') {
              steps {
                  sh "docker-compose -f docker-compose.yml up -d  "
              }
              }
        stage('Sending email'){
           steps {
            mail bcc: '', body: '''Hello from Jenkins,
            Devops Pipeline returned success.
            Best Regards''', cc: '', from: '', replyTo: '', subject: 'Devops Pipeline', to: 'ghaith.belhadjsghaier@esprit.tn'
            }
       }

    }
}
