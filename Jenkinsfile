pipeline {
  agent any
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
                 sh 'docker tag inventory-mgmt-service:latest 092390458462.dkr.ecr.us-west-2.amazonaws.com/inventory-mgmt-service'
                 sh 'docker push 092390458462.dkr.ecr.us-west-2.amazonaws.com/inventory-mgmt-service'

                 sh 'docker tag inventory-management-items:latest 092390458462.dkr.ecr.us-west-2.amazonaws.com/inventory-management-items'
                 sh 'docker push 092390458462.dkr.ecr.us-west-2.amazonaws.com/inventory-management-items'
              }
    }
  }
}
