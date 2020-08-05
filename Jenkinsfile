pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/nutmag/dvja.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
	stage ('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: 'scan = "https://github.com/nutmag/dvja.git" --format ALL', odcInstallation: 'Dependency-check 5.3.2'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
	  
    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn clean package'
          sh 'mvn sonar:sonar \
              -Dsonar.projectKey=dvja \
              -Dsonar.host.url=http://vast.fruxlabs.com:9000 \
              -Dsonar.login=61829e4a1d9e43f1af407af8f7f870588e8e58f7'
        }
      }
    }
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
   stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['csserver']) {
                sh 'scp -P 2200 -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/dvja/target/*.war csserver@62.171.148.41:/opt/tomcat/apache-tomcat-8.5.57/webapps/'
              }      
           }       
    	}	  
   	  
  }
}
