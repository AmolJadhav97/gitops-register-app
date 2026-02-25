pipeline{
    agent any
    environment{
        APP_NAME = "register-app-pipeline"
    }

    
    stages{
        stage("cleanup"){
            steps {
            cleanWs()
            }
        }
        stage("checkout from scm"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/AmolJadhav97/gitops-register-app'
            }    
        }
        stage("update deployment tags"){
            steps{
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to github"){
            steps{
                sh """
                   git config --global user.name "amoljadhav97"
                   git config --global user.email "007asjadhav0@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/amoljadhav97/gitops-register-app main"
                }
            }
        }
    }
}
