pipeline {
    agent any


    environment {

    DOCKERHUB_CREDENTIALS = credentials('dockerhubuser')
     registry = "srikanthyadavalli/lms-f-e"
        registryCredential = 'dockerhubuser'
    }

    stages {

        
         stage('Building the docker fe image') {
            steps {
                sh 'cd webapp && docker build -t srikanthyadavalli/lms-f-e .'
            }
        }
        stage('Logging into dockerhub account') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('pushing the docker fe image into dockerhub') {
            steps {
                  sh 'docker push srikanthyadavalli/lms-f-e'
            }
        }
        stage('Remove old docker fe images') {
             steps {
                 sh 'docker rmi -f srikanthyadavalli/lms-f-e'
            }
        }
        stage('Building the docker be image') {
            steps {
                sh 'cd api && docker build -t srikanthyadavalli/lms-b-e .'
            }
        }
        
        stage('pushing the docker be image into dockerhub') {
            steps {
                  sh 'docker push srikanthyadavalli/lms-b-e'
            }
        }
        stage('Remove old docker be images') {
             steps {
                 sh 'docker rmi -f srikanthyadavalli/lms-b-e'
            }
        }
         stage('Running the docker containers') {
            steps {
                  sh 'cd k8s && kubectl apply -f lms-postgres-secret.yaml -f lms-backend-cm.yaml -f lms-postgres-deployment.yaml -f lms-postgres-svc.yaml -f lms-backend-deployment.yaml -f lms-backend-svc.yaml -f lms-frontend-deployment.yaml -f lms-frontend-svc.yaml'
            }
        }
    }
}
