pipeline {
       agent any
  tools {
    maven 'Maven'
  }
  environment {
   registry = "raghavgeek/testing"
   registryCredential = "8acfc31c-d902-463d-ad29-afdc446892df"
  }
  stages {
    stage('Initialize'){
      steps{
        echo "We are doing some test"
        echo "PATH = ${PATH}"
        }
    }
    stage('Build'){
           steps
           {
        sh "mvn clean install"
    }     
    }
         stage('Sonar'){
                steps
                {
        
            sh "mvn sonar:sonar -Dsonar.projectKey=SpringPipeline -Dsonar.host.url=http://52.143.7.186/sonarqube-1336430 -Dsonar.login=23e2ff1beac97da72a5edff2c7e3a72e33578244"
                }
     }
         
         
stage('Building/Deploying our image') { 
       steps
       {
             withCredentials([usernamePassword(credentialsId: '8acfc31c-d902-463d-ad29-afdc446892df', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh ("docker login -u ${USERNAME} -p ${PASSWORD}")
                        sh ("docker build -t ${USERNAME}/testing:first .")
                        sh ("docker push ${USERNAME}/testing:first")
                }
     }
}
         
  stage("Deploy on AKS using cluster"){
              steps { 
    	            sshagent(['raghava_1336430']){
    	            sh "scp -o StrictHostKeyChecking=no spring.yaml raghava_1336430@13.93.120.161:/home/raghava_1336430/"
                   script
                          {
                              sh "ssh raghava_1336430@13.93.120.161 kubectl create -f . -n devopss-1336430"   
                          }
                   }
    	}
    }
      
}
}
