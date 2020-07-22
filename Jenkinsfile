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
    
    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn clean package'
          sh 'mvn sonar:sonar \
              mvn sonar:sonar \
              mvn sonar:sonar \
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
  }
}
