pipeline {
    {
      label "jenkins-slave"
  }
    stages {
        stage('build') {
            steps {
                script {
                      sh """
                            docker bulid gcr.io/mercurial-time-233114/penguin:${BUILD_NUMBER} .
                            docker push gcr.io/mercurial-time-233114/penguin:${BUILD_NUMBER}
                       """
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                  
                  sh """
                      kubectl apply -f Deployment/.
                    """
                   
                }
            }
        }
    }
}
