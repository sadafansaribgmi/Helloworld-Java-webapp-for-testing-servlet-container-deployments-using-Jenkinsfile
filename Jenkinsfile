pipeline {
    agent any
    tools {
        maven 'maven' 
    }
    stages {
        stage("Test") {
            steps {
		sh 'mvn test'
		sh 'mvn --version'
		slackSend channel: 'montranjenkins', message: 'Job started'
            }
        }
        stage("Build") {
            steps {
                sh 'mvn package'
            }
        }
        stage(" Deploy on Test") {
            steps {
		deploy adapters: [tomcat7(credentialsId: 'tomcat', path: '', url: 'http://10.2.0.32:8080')], contextPath: '/app', war: '**/*.war'
            }
        }
        stage("Deploy on Prod") {
            input{
	        message "Should we continue?"
		ok "Yes we should"
	    }
	    steps {
		deploy adapters: [tomcat7(credentialsId: 'tomcat', path: '', url: 'http://10.2.0.33:8080')], contextPath: '/app', war: '**/*.war'
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

