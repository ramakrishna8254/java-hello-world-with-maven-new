pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-3.6.3/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/ramakrishna8254/java-hello-world-with-maven-new.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean install'
		    sh 'docker build -t dockerfile .'
            }
         }
	    stage('Sonar_Analysis'){
	     steps{
                      script{
                      withSonarQubeEnv('sonarserver') { 
                      sh "mvn sonar:sonar"
                       }
                      timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
		    sh "mvn clean install"
                  }
                }  
              }
    	}
    }
