pipeline {
   agent any

   environment {
     // You must set the following environment variables
      SERVICE_NAME = "fleetman-webapp"
      ORGANIZATION_NAME = "kudadigital"
      YOUR_DOCKERHUB_USERNAME = "augeos" //(it doesn't matter if you don't have one)
      REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
      registry = ${REPOSITORY_TAG}
      dockerImage = ''
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh 'echo No build required for Webapp.'
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
         }
      }

      stage('Push Image to Registry') {
         steps {
            script {
               docker.withRegistry('', credentialsId: 'DockerHub') {
                  dockerImage.push()
               }
            }
         }
      }

      stage('Deploy to Cluster') {
          steps {
            sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}
