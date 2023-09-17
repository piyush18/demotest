node{
    stage("Pull Source code from Git"){
    git 'https://github.com/piyush18/demotest.git'
    }
    stage('docker image')
    {
        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID piyushsharma8860/$JOB_NAME:v1.$BUILD_ID'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID piyushsharma8860/$JOB_NAME:latest'
    }
    stage('Push Image to Docker Hub'){
      withCredentials([string(credentialsId: 'dockerhubpasswordpiyush', variable: 'dockerhubpasswordpiyush')]) {
       sh 'docker login -u piyushsharma8860 -p ${dockerhubpasswordpiyush}'
       sh 'docker image push piyushsharma8860/$JOB_NAME:v1.$BUILD_ID'
       sh 'docker image push piyushsharma8860/$JOB_NAME:latest'
       sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID piyushsharma8860/$JOB_NAME:v1.$BUILD_ID piyushsharma8860/$JOB_NAME:latest'
        }
    }
    stage('Deployment of Docker Container'){
         def dockerrun='docker run -p 8080:8080 -d --name cloudpiyush piyushsharma8860/grovypipeline:latest' 
       sshagent(['piyushtest']) {
    // some block
          sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.14.125 ${dockerrun}"

      
       }
        
    }
}
