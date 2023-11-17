pipeline {
    agent any


    environment {

    DOCKERHUB_CREDENTIALS = credentials('dockerhubuser')
     registry = "srikanthyadavalli/lms-f-e"
        registryCredential = 'dockerhubuser'
    }

    stages {

        
         stage('Building the docker image') {
            steps {
                sh 'cd webapp && docker build -t srikanthyadavalli/lms-f-e .'
            }
        }
        stage('Logging into dockerhub account') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('pushing the docker image into dockerhub') {
            steps {
                  sh 'docker push srikanthyadavalli/lms-f-e'
            }
        }
        stage('Remove old docker images') {
             steps {
                 sh 'docker rmi -f srikanthyadavalli/lms-f-e'
            }
        }
         stage('Running the docker container') {
            steps {
                  sh 'cd k8s && kubectl apply -f lms-frontend-deployment.yaml'
            }
        }
    }
}
