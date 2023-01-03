pipeline {
    agent any
	
	  tools
    {
       maven "maven-3.6.3"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/ssgrandhi/CI-CD-using-Docker1.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'sudo docker build -t samplewebapp:latest .' 
                sh 'sudo docker tag samplewebapp vvsatyagupta/samplewebapp:latest'
                sh 'sudo docker tag samplewebapp vvsatyagupta/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "docker1", url: "https://index.docker.io/v1/" ]) {
          sh  'sudo docker push vvsatyagupta/samplewebapp:latest'
        //  sh  'docker push nikhilnidhi/samplewebapp:$BUILD_NUMBER' 
        }
                 
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "sudo docker run -d -p 8004:8080 vvsatyagupta/samplewebapp"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
	       sshagent(['clientserver']) {
               sh 'ssh -o StrictHostKeyChecking=no -l ec2-user 172.31.84.29 uname -a'
                sh "sudo docker -H ssh://ec2-user@172.31.82.171 run -d -p 8004:8080 vvsatyagupta/samplewebapp"
 
            }
        }
    }
	}
    
