def dockeruser = "ravitejademolab"
def imagename = "webserver-image"
def container = "webserver"

node {
   echo 'Building Nginx Docker Image'

stage('Git Checkout') {
    git 'https://github.com/teja190827/teja190827'
    }
    
stage('Build Docker Image'){
     sh label: '', script: "docker build -t ${imagename} ."
    }

stage('Stop Existing Container'){
     sh label: '', script: "docker stop ${container}"
    }	

stage ('Running Container to test built Docker Image'){
    sh label: '', script: "docker run -dit --rm --name ${container} -p 80:80 ${imagename}"
    }
    
stage('Tag Docker Image'){
    sh label: '', script: "docker tag ${imagename} ${dockeruser}/${imagename}:v1"
    }

stage('Docker Login and Push  Image'){
    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
    sh label: '', script: "docker login -u ${USERNAME} -p ${PASSWORD}"
    }
    sh label: '', script: "docker push  ${dockeruser}/${imagename}:v1"
    }
}