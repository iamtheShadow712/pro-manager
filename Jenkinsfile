pipeline{
    agent any;
    stages{
        stage("Develop Branch"){
            when{
                branch "develop"
            }
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