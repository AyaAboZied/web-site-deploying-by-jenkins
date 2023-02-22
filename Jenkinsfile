pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                script {
                   withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                       sh """
                            docker login -u $USERNAME -p $PASSWORD
                            docker build -t ayaabdelmomen/penguin:${BUILD_NUMBER} .
                            docker push ayaabdelmomen/penguin:${BUILD_NUMBER}
                       """
                   }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                   withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]){
                  sh """
                      mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                      cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                      rm -f Deployment/deploy.yaml.tmp
                      kubectl apply -f Deployment --kubeconfig=${KUBECONFIG}
                    """
                   }
                }
            }
        }
    }
}
