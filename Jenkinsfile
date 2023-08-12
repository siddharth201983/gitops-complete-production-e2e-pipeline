pipeline {
    agent{
        label "jenkins-agent"
    }
    environment{
        APP_NAME = "java-demoapp"
        RELEASE = "1.0.0"
        DOCKER_USER = "siddharth20"
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage("Checkout from SCM"){
            steps{
                script{
                    git branch: 'main', credentialsId: 'gitcred', url: 'https://github.com/siddharth201983/gitops-complete-production-e2e-pipeline.git'
                }
            }
        }
        stage("Update the Deployment Tags"){
            steps{
                script{
                    sh '''
                        cat deployment.yaml
                        sed -i 's/${IMAGE_NAME}:*/${IMAGE_NAME}:${IMAGE_TAG}/g' deployment.yaml
                        cat deployment.yaml
                    '''
                }
            }
        }
        stage("Push the changed deployment file to git"){
            steps{
                script{
                    sh '''
                        git config --global user.name "siddharth201983"
                        git config --global user.email "sharma.siddharth2009@gmail.com"
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest"
                    '''
                    withCredentials([gitUsernamePassword(credentialsId: 'gitcred', gitToolName: 'Default')]){
                        sh "git push https://github.com/siddharth201983/gitops-complete-production-e2e-pipeline.git main"
                    }
                }
            }
        }
    }
}
