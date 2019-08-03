pipeline {
  agent any
  
  tools {
    jdk 'JAVA_HOME'
    maven 'MAVEN_HOME'
}
  stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace... */
      steps {
        checkout scm
      }
    }
    stage('Build Project and Generate Docker Images') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        sh 'echo $USER'
        sh 'echo whoami'
      }
    }
    stage('Push images to aws ecr'){
              steps {
                withDockerRegistry(credentialsId: 'ecr:ap-south-1:aws-creds', url: '242415084315.dkr.ecr.ap-south-1.amazonaws.com/inventory-mgmt-service') {
                 sh 'docker tag inventory-mgmt-service:latest 242415084315.dkr.ecr.ap-south-1.amazonaws.com/inventory-mgmt-service'
                 sh 'docker push 242415084315.dkr.ecr.ap-south-1.amazonaws.com/inventory-mgmt-service'

                 sh 'docker tag inventory-management-items:latest 242415084315.dkr.ecr.ap-south-1.amazonaws.com/inventory-management-items'
                 sh 'docker push 242415084315.dkr.ecr.ap-south-1.amazonaws.com/inventory-management-items'
            }
              }
    }
    stage('Run docker images on kubernetes cluster') {
              steps {
                node('prod'){
                checkout scm
                 sh 'kubectl apply -f deployment.yaml'
                 sh 'kubectl apply -f service.yaml'
                 sh 'kubectl apply -f ingress.yaml'
                }
              }
            }
  }
}
