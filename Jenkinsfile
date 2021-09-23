pipeline {
    agent any
    environment {
        PROJECT_ID = 'mobility-320606'
        CLUSTER_NAME = 'cluster-1'
        LOCATION = 'asia-east2-a'
        CREDENTIALS_ID = 'mobility-320606'
    }
    stages {
        stage('Deploy to GKE') {
             steps{
            //     step([
            //     $class: 'KubernetesEngineBuilder',
            //     projectId: env.PROJECT_ID,
            //     clusterName: env.CLUSTER_NAME,
            //     location: env.LOCATION,
            //     manifestPattern: 'manifest.yaml',
            //     credentialsId: env.CREDENTIALS_ID,
            //     verifyDeployments: true])
            
            echo "Test"
            }
            
        }
    }
}