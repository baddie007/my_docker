pipeline {
       agent any
  tools {
    maven 'Maven'
  }
  environment {
   registry = "arpit74/mytest"
   registryCredential = "638c46c6-0260-4af4-adee-fe5d19c229ec"
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
         
         
stage('Building/Deploying our image') { 
       steps
       {
             withCredentials([usernamePassword(credentialsId: '638c46c6-0260-4af4-adee-fe5d19c229ec', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh ("docker login -u ${USERNAME} -p ${PASSWORD}")
                        sh ("docker build -t ${USERNAME}/mytest:first .")
                        sh ("docker push ${USERNAME}/mytest:first")
                }
     }
}
         
  stage("Deploy on AKS using cluster"){
              steps { 
    	            sshagent(['arpit_1645386']){
    	            sh "scp -o StrictHostKeyChecking=no spring.yaml arpit_1645386@13.93.120.161:/home/arpit_1645386/"
                   script
                          {
                                 try
                                 {
                              sh "ssh arpit_1645386@13.93.120.161 kubectl apply -f spring.yaml -n arpit1645386"   
                                 }
                                 catch(error)
                                 {
                                 sh "ssh arpit_1645386@13.93.120.161 kubectl create -f spring.yaml -n arpit1645386"   
                                 }
                   }
    	}
    }
  }
}
}
