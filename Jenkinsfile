pipeline{
    agent any;
    tools {
        nodejs 'nodejs-v24.0.0'
    }
    environment {
        DOCKER_CREDS = credentials("dockerhub-creds")
        DOCKER_IMAGE_NAME = "${DOCKER_CREDS_USR}/jenkins-client:${GIT_COMMIT}"
    }
    stages{
        stage("Check for Git Tag"){
            steps{
                script{
                    def tag = sh(returnStdout: true, script: 'git tag --contains').trim()
                    if (tag != null){
                        env.GIT_TAG = tag
                    }else{
                        env.GIT_TAG = ''
                    }
                    echo "Git tag is set to: ${env.GIT_TAG}"
                    env.IMAGE_RELEASE_TAG = "${DOCKER_IMAGE_NAME}:${GIT_TAG}"
                }
            }
        }
        // stage("Setup"){
        //     steps{
        //         dir('./client'){
        //             sh "npm install"
        //         }
        //     }
        // }
        stage("Build and Deploy"){
            // when{
            //     expression{
            //         return env.GIT_TAG != ""
            //     }
            // }
            stages{
                stage("Build"){
                    steps{
                        dir("./client"){
                            sh "docker build -t ${DOCKER_IMAGE_NAME} ."
                            echo "docker image build successfully"
                        }
                    }
                }
                stage("Push to DockerHub"){
                    steps{
                        withDockerRegistry(credentialsId: 'dockerhub-creds') {
                            sh "docker push ${DOCKER_IMAGE_NAME}"
                            echo "Pushed to docker hub"
                        }
                    }
                }
            }
        }
    }
}