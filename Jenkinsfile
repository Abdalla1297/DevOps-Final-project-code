pipeline {
    agent any  
    stages {
        stage('build') {
            steps {
                       withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                           sh """
                                docker login -u $USERNAME -p $PASSWORD
                                docker build -t bojy1297/bakehouse:v${BUILD_NUMBER} .
                                docker push bojy1297/bakehouse:v${BUILD_NUMBER}
                           """
                       }
                    }
                }
        stage('deploy') {
            steps {
                            withCredentials([file(credentialsId: 'accesscluster', variable: 'config')]) {
                          sh """
                              sed -i 's/tag/${BUILD_NUMBER}/g' Deployment/deploy.yaml
                              gcloud auth activate-service-account --key-file=${config}
                              gcloud container clusters get-credentials private-standerd-gke-cluster --zone us-central1-b --project abdallah-iti-377721
                              kubectl apply -f Deployment
                            """
                        }
                    }
                }
        }
}