pipeline {
    agent any 

    environment {
        GIT_REPO_NAME = "deployment-file"
        GIT_USER_NAME = "sheersagar"
    }

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
                    withCredentials([usernamePassword(credentialsId: 'git-cred', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')]) {
                        sh """
                        git config user.email "vishavdeshwal@gmail.com"
                        git config user.name "vishav deshwal"
                        cat deployment.yaml
                        echo "IMAGE_NAME IS: ${params.IMAGE_NAME}"
                        sed -i 's|image: .*|image: ${params.IMAGE_NAME}|' deployment.yaml
                        cat deployment.yaml
                        git add deployment.yaml
                        git commit -m "updated the deployment file"
                        git push https://$GIT_USER:$GIT_TOKEN@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME}.git HEAD:main
                        """
                    }
                }
            }
        }
    }
}