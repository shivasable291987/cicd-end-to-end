pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
              git branch: 'main', credentialsId: 'Gitpassword', url: 'https://github.com/shivasable291987/cicd-end-to-end.git'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t shivsable29/cicdend2end:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
               withDockerRegistry(credentialsId: 'dockerhub', url: 'https://hub.docker.com/repository/docker/shivsable29/cicdend2end/') {
    // some block

                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push shivsable29/cicdend2end:${BUILD_NUMBER}
                    '''
                }
           }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git branch: 'main', credentialsId: 'Gitpassword', url: 'https://github.com/shivasable291987/CICD-MANIFEST.git'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([gitUsernamePassword(credentialsId: 'Gitpassword', gitToolName: 'Default')]) {
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/shivasable291987/CICD-MANIFEST.gitt HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
}
