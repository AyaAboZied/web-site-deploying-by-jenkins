pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                script {
                   withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                       sh """
                            docker login -u $USERNAME -p $PASSWORD
                            docker build -t ayaabdelmomen/penguin .
                            docker push ayaabdelmomen/penguin
                       """
                   }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                  sh """
                      mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                      cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                      rm -f Deployment/deploy.yaml.tmp
                      kubectl apply -f Deployment -n default
                    """
                }
            }
        }
    }
}
