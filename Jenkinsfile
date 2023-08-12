pipeline {
    agent {
        label "java-demoapp"
    }
    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
    
    
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'gitcred', url: 'https://github.com/siddharth201983/gitops-complete-production-e2e-pipeline'
            }
        }
    

    
        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "siddharth201983"
                    git config --global user.email "sharma.siddharth2009@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'gitcred', gitToolName: 'Default')]) {
                    sh "git push https://github.com/siddharth201983/gitops-complete-production-e2e-pipeline main"
                }
            }
        }

    }

}