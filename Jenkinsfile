pipeline{
    agent{
        label "Jenkins-slave"
    }
    environment{
        // the app name should be a lowercase otherwise the pipline will return an error 
        APP_NAME="java_app"
    }
    stages{
        // ensures that there are no residual artifacts or files from previous build
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        //get the code from github repo!!
        stage("Checkout from the SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Saifweb/deploy.git'
                }
        }
        // Update the Deployement Tags
        stage("Update Deployement Tag"){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                   """
                }
        }
        // Push the changed deployement file to github
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "Saifweb"
                   git config --global user.email "saifbenhmida1@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Saifweb/deploy.git main"
                }
            }
        }
    }
}