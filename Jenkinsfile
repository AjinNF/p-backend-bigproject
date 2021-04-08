env.DOCKER_REGISTRY = 'ajinnf'
env.DOCKER_IMAGE_NAME = 'backend'
node('master') {
    stage('HelloWorld') {
      echo 'Hello World!'
    }
    stage('Git Clone from Github') {
        git url: 'https://github.com/AjinNF/p-backend-bigproject.git' 
    }
    stage('Build Docker Image') {    
        sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."   
    }
    stage('Push Docker Image to Docker Hub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('Deploy To Kubernetes Cluster') {
        sh'''sed -i "15d" backend-deployment.yml'''
        sh'''sed -i "14 a \'\\'          image: ajinnf/backend:${BUILD_NUMBER}" backend-deployment.yml && sed -i "s/''//"  backend-deployment.yml'''
        sh "kubectl apply -f  backend-deployment.yml"
   }
    stage('Remove Docker Image in local') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"   
    }
    stage("Clean Workspace"){
        cleanWs()
    }    
}
