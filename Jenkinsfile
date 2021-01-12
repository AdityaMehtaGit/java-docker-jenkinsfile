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
    
                    sh 'docker build -t java-docker/hello-world:latest .'
                    // withCredentials([usernamePassword(passwordVariable : 'DOCKER_PASSWORD' ,usernameVariable : 'DOCKER_USERNAME' ,credentialsId : "$DOCKER_CREDENTIAL_ID" ,)]) 
                    //  {
                    //     sh 'echo "$DOCKER_PASSWORD" | docker login $REGISTRY -u "$DOCKER_USERNAME" --password-stdin'
                    //     sh 'docker push  $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER'
                    //  }
                 
            }
        }

        stage ('Push image to Artifactory') { // take that image and push to artifactory
        steps {

rtServer (
    id: 'jfrog',
    url: 'http://localhost:8082/artifactory',
    // If you're using username and password:
    // username: 'admin',
    // password: 'Aditya@mehta1',
    // If you're using Credentials ID:
    credentialsId: '8a999032-067a-4a43-9036-3675f3676437',
    // If Jenkins is configured to use an http proxy, you can bypass the proxy when using this Artifactory server:
    bypassProxy: true,
    // Configure the connection timeout (in seconds).
    // The default value (if not configured) is 300 seconds:
    timeout: 300
)
            rtDockerPush(
                serverId: "jrog",
                image: "java-docker/hello-world:latest",
                // image: ARTIFACTORY_DOCKER_REGISTRY + '/hello-world:latest',
                // Host:
                // On OSX: 'tcp://127.0.0.1:1234'
                // On Linux can be omitted or null
                host: 'unix:///var/run/docker.sock',
                targetRepo: 'java-docker', // where to copy to 
                // Attach custom properties to the published artifacts:
                properties: 'project-name=docker;status=stable'
                // If the build name and build number are not set here, the current job name and number will be used:
                // buildName: 'my-build-name',
                // buildNumber: '17', 
            )
             rtPublishBuildInfo (
                 serverId: 'jfrog'
            )

            }
        }
    }

}