pipeline {
 agent any

 stages {

  stage('Clone') {
   steps {
    git branch: 'main',
    credentialsId: 'github',
    url: 'https://github.com/Arifjmi/k8s-cicd.git'
   }
  }

  stage('Build Docker Image') {
   steps {
    sh 'docker build -t arifreza/webapp:latest .'
   }
  }

  stage('Push Image') {
   steps {
    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
     sh '''
     echo $PASS | docker login -u $USER --password-stdin
     docker push arifreza/webapp:latest
     '''
    }
   }
  }

  stage('Deploy to Kubernetes') {
   steps {
    sh '''
    kubectl set image deployment/web web=arifreza/webapp:latest -n cicd
    '''
   }
  }
 }
}
