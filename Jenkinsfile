pipeline{
    agent any;
    tools {
        nodejs 'nodejs-v24.0.0'
    }
    stages{
        stage("Develop Branch"){
            when{
                branch "develop"
            }
            stages{
                stage("Install Dependencies"){
                
                    steps{
                        dir("./client"){
                            sh "npm install --audit=false"
                        }
                    }
                }

                stage("Run Audit"){
                    steps{
                        dir("./client"){
                            sh "npm audit --audit-level=high"
                        }
                    }
                }
            }
        }
    }
}