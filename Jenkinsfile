pipeline {
environment {
    PROJECT = "mobility-320606"
    APP_NAME = "gceme"
    FE_SVC_NAME = "${APP_NAME}-frontend"
    CLUSTER = "app-cluster"
    CLUSTER_ZONE = "asia-east2-a"
    IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    JENKINS_CRED = "${PROJECT}"
 }
agent {
        kubernetes {
            label 'jenkins-agent'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  name: jenkins-agent
labels:
        component: ci
spec:
  containers:
  - name: golang
    image: golang:1.12
    command:
    - cat
    tty: true
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
        }
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'skip git checkout'
                // Clone repository
                //git 'https://github.com/cgn170/sample-go-http-app'
            }
        }

        stage('Build Golang'){
            steps {
                container(name:'golang') {
                    // Get dependencies
                    sh "go get github.com/gorilla/mux"
                    sh "go mod init"


                     sh """
                            ln -s `pwd` /go/src/sample-app
                            cd /go/src/sample-app
                            go test
                        """
                    // Build Golang project    
                    sh "go build -o sample ."
                }
            }
        }

        stage('Build Image Kaniko') {
            steps {
                container(name:'kaniko') {
                    
                    // Build image without push to repository
                    // sh '''executor \
                    //       --no-push \
                    //       --context $WORKSPACE \
                    //       --dockerfile $WORKSPACE/Dockerfile 
                    // '''
                    
                    //Build and push image to repository
                    
                    //sh '''executor \
                    //      --context $WORKSPACE \
                    //      --dockerfile $WORKSPACE/Dockerfile \
                    //      --destination $REGISTRY/$REPOSITORY/$IMAGE
                    //'''

                    sh '''executor \
                         --context $WORKSPACE \
                         --dockerfile $WORKSPACE/Dockerfile \
                         --destination <gcr.io/mobility-320606/APP_NAME:${env.BRANCH_NAME}>
                    '''


                  
                        //   args: ["--dockerfile=<path to Dockerfile within the build context>",
                        //          "--context=dir://<path to build context>",
                        //          "--destination=<gcr.io/$PROJECT/$IMAGE:$TAG>"]

                    /*
                    Authentication is required to push an image to Google Container Registry,
                    find further documentation in Kaniko repository:
                    https://github.com/GoogleContainerTools/kaniko#pushing-to-google-gcr 
                    */
                }
            }
        }
    }
}
