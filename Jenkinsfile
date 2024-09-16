pipeline {
  agent any
  tools{
    maven 'Maven'
  }
  stages {
    stage ('Initialize'){
      steps {
        sh '''
              echo "PATH = ${PATH}"
              echo "M2_HOME = ${M2_HOME}"
          '''
            } 
    }
    stage ('Build'){
      steps {
        sh 'mvn clean package'       
      }  
    }

  stage ('Deploy-to-Tomcat'){
      steps {
        sshagent(['tomcat-ssh']){
        sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.232.190.69:/prod/apache-tomcat-10.1.29/webapps/webApp.war'

        }
        }  
    }
  
  
  
  
  
  
  
  
  } 
}
