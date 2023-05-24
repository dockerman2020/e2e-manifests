pipeline {
    agent {
        label "macos"
    }
    environment {
        APP_NAME = "e2e-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
    
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/dockerman2020/e2e-manifests'
            }
        }
    
        stage("Update the Deployment Tags") {
            steps {
                sh """
                    sed 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deploy.yaml >deploy-1.yaml
                    mv deploy-1.yaml deploy.yaml
                """
            }
        }

        stage("Push the changed deploy file to Git") {
            steps {
                sh """
                    git config --global user.name "dockerman2020"
                    git config --global user.email "75863443+dockerman2020@users.noreply.github.com"
                    git add deploy.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'e2e-pat', gitToolName: 'Default')]) {
                    sh "git push https://github.com/dockerman2020/e2e-manifests main"
                }
            }
        }

    }

}