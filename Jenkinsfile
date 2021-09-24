pipeline {
agent {
        kubernetes {
            label 'jenkins-agent'
            yaml """
                kind: Pod
                metadata:
                name: jenkins-agent
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
                """
        }
    }

    stages {
        stage('Checkout - Go testing') {
            steps {
                // Clone repository
                //git 'https://github.com/cgn170/sample-go-http-app'
                sh """
                    ln -s `pwd` /go/src/sample-app
                    cd /go/src/sample-app
                    go test
                """
            }
        }

        stage('Build Golang'){
            steps {
                container(name:'golang') {
                    // Get dependencies
                    sh "go get github.com/gorilla/mux"
                    // Build Golang project    
                    sh "go build -o sample ."
                }
            }
        }

        stage('Build Image Kaniko') {
            steps {
                container(name:'kaniko') {
                    // Build image without push to repository
                    sh '''executor \
                          --no-push \
                          --context $WORKSPACE \
                          --dockerfile $WORKSPACE/Dockerfile 
                    '''

                    /*
                    Build and push image to repository
                    
                    sh '''executor \
                          --context $WORKSPACE \
                          --dockerfile $WORKSPACE/Dockerfile \
                          --destination $REGISTRY/$REPOSITORY/$IMAGE
                    '''
                    Authentication is required to push an image to Google Container Registry,
                    find further documentation in Kaniko repository:
                    https://github.com/GoogleContainerTools/kaniko#pushing-to-google-gcr 
                    */
                }
            }
        }
    }
}
