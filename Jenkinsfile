pipeline {
    agent any
    
    environment {
        SERVICE_NAME = "class-cohort-1"
        ORGANIZATION_NAME = "afrihub"
        DOCKERHUB_USERNAME = "dnerorepo"
        REPOSITORY_TAG = "${DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"

    }
    
    stages {

        
        stage ('Build and Push Image') {
            steps {
                 withDockerRegistry([credentialsId: 'dockerIDCred', url: ""]) {
                   sh 'docker build -t ${REPOSITORY_TAG} .'
                   sh 'docker push ${REPOSITORY_TAG}'        
            }
          }
       }
        
       // stage("Install kubectl"){
       //      steps {
       //          sh """
       //              curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
       //              chmod +x ./kubectl
       //              ./kubectl version --client
       //          """
       //      }
       //  }
        
        stage ('Deploy to Cluster') {
            steps {
                sh "aws eks update-kubeconfig --region eu-west-1 --name ekscluster"
                sh " envsubst < ${WORKSPACE}/deploy.yaml | ./kubectl apply -f - "
            }
        }
    }
}
