pipeline {
    agent any
    // agent {
    //     kubernetes {
    //     label 'sample-app'
    //     defaultContainer 'jnlp'
    //     yaml """
    //         apiVersion: v1
    //         kind: Pod
    //         metadata:
    //         labels:
    //             component: ci
    //         spec:
    //             # Use service account that can deploy to all namespaces
    //             serviceAccountName: jenkins
    //             containers:
    //             - name: golang
    //             image: golang:1.10
    //             command:
    //             - cat
    //             tty: true
    //             - name: gcloud
    //             image: gcr.io/cloud-builders/gcloud
    //             command:
    //             - cat
    //             tty: true
    //             - name: kubectl
    //             image: gcr.io/cloud-builders/kubectl
    //             command:
    //             - cat
    //             tty: true
    //         """
    //     }
    // }
    environment {
        PROJECT_ID = 'mobility-320606'
        CLUSTER_NAME = 'cluster-1'
        LOCATION = 'asia-east2-a'
        CREDENTIALS_ID = 'mobility-320606'
    }
    stages {
        stage('Deploy to GKE') {
             steps{
                step([
                $class: 'KubernetesEngineBuilder',
                projectId: env.PROJECT_ID,
                clusterName: env.CLUSTER_NAME,
                location: env.LOCATION,
                manifestPattern: 'k8s/canary/',
                credentialsId: env.CREDENTIALS_ID,
                verifyDeployments: true])
            
            echo "Test"
            }
            
        }
        
    }
}