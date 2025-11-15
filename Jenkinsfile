pipeline{
    agent any;
    tools {
        nodejs 'nodejs-v24.0.0'
    }
    stages{
        stage("Develop Branch"){
            when{
                branch "develop"
                beforeAgent true
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
        stage("Pull Request: develop -> main"){
            when {
                changeRequest branch: 'develop', target: 'main'
                beforeAgent true
            }
            stages{
                stage("Pull Request Number"){
                    steps{
                        sh '''
                            env
                        '''
                    }
                }
                stage("Install Dependencies"){
                    steps{
                        sh '''
                            npm install --no-audit
                        '''
                    }
                }
                stage("Audit Dependencies"){
                    steps{
                        sh '''
                            npm audit --audit-level=high
                        '''
                    }
                }
            }
        }
        // stage("")
    }
}