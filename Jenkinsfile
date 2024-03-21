pipeline {
    agent any 

    parameters{
        string(name: 'IMAGE_NAME', defaultValue: '', description: 'Docker image name to deploy')
    }

    stages {

        stage('Checkout stage') {
            steps {
                git branch: 'main', changelog: false, credentialsId: 'git-cred', poll: false, url: 'https://github.com/sheersagar/deployment-file.git'
            }
        }

        stage('Update the deployment file and push it') {
            steps{
                script {
                    sh "sed -i 's|image:.* |image:${params.IMAGE_NAME}|' deployment.yaml"
                    sh '''
                    git config user.name "vishav deshwal"
                    git config user.email "vishavdeshwal@gmail.com"
                    git add deployment.yaml
                    git commit -m "updated the deployment file"
                    git push origin main

                    '''
                }
            }
        }
    }
}