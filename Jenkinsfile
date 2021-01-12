pipeline {
    agent any
    environment {
        registry = "AdityaMehtaGit/java-docker-jenkins"
        DOCKER_CREDENTIAL_ID = 'dockerhub-id'
        GITHUB_CREDENTIAL_ID = 'github-id'
        // KUBECONFIG_CREDENTIAL_ID = 'demo-kubeconfig'
        REGISTRY = 'docker.io'
        APP_NAME = 'HelloWorld'
    }

    stages {

        stage ('checkout scm') {
            steps {
                checkout(scm)
            }
        }
        stage ('pkg-build') {
            steps {
                    sh 'mvn clean install'
                    sh 'mvn clean package'
            }
        }
        stage ('docker-build') {
            steps {
    
                    sh 'docker build -t $REGISTRY/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER .'
                    // withCredentials([usernamePassword(passwordVariable : 'DOCKER_PASSWORD' ,usernameVariable : 'DOCKER_USERNAME' ,credentialsId : "$DOCKER_CREDENTIAL_ID" ,)]) 
                    //  {
                    //     sh 'echo "$DOCKER_PASSWORD" | docker login $REGISTRY -u "$DOCKER_USERNAME" --password-stdin'
                    //     sh 'docker push  $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER'
                    //  }
                 
            }
        }

    }

}