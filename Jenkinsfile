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
             steps { 
                    sh "docker build -t raghavgeek/testing:first ."
                    sh "docker push raghavgeek/testing"
                 } 

     }
         
  
       stage('Deploy Application on K8s') {
              steps
              {
                sh("curl -LO https://storage.googleapis.com/kubernetes-release/release/\$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl")
                sh("chmod +x ./kubectl")
                sh("cat ./spring.yaml | ./kubectl apply -f -")
                echo "Application started on port: HTTP_PORT (http)"
    }
       }
}
}
