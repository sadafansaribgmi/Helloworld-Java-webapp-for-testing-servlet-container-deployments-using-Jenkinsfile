pipeline {
    agent any
    tools {
        maven 'Maven3' 
    }
    stages {
        stage("Test") {
            steps {
		sh 'mvn test'
		sh 'mvn --version'
            }
        }
        stage("Build") {
            steps {
                sh 'mvn package'
            }
        }
        stage(" Deploy on Test") {
            steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcatserver1', path: '', url: 'http://13.233.138.186:8080')], contextPath: '/app', war: '**/*.war'
	    }
        }

    }
	post{
	    always{
		 echo "=========always====="
		} 
            success{
		  echo "=========pipeline executed successfully====="
		  slackSend channel: 'montranjenkins', message: 'Job Success'
		}
            failure{
	      echo "=========pipeline execution failed====="
	      slackSend channel: 'montranjenkins', message: 'Job failed'
		}		
	
	}
}

