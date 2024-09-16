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
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog-scan-results||true'
        sh 'docker run gesellix/trufflehog --json https://github.com/AJwhitewalker/webapp.git > trufflehog-scan-results'
        sh 'cat trufflehog-scan-results'   
      }
    }

     stage ('Software-Composition-Analysis') {
      steps {
        sh 'rm owasp||true'
        sh 'wget "https://github.com/AJwhitewalker/webapp/owasp-dependency-check.sh"'
        sh 'chmod +x owasp-dependency-check.sh'
        sh 'bash owasp-dependency-check.sh'   
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
        sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.232.190.69:/home/ubuntu/prod/apache-tomcat-10.1.29/webapps/webApp.war'

        }
        }  
    }
  
  
  
  

  
  } 
}
