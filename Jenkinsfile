pipeline {
    agent {label 'jenkins-slave'}

    stages {
        stage('test') {
            steps {
                echo 'test'
                sh "ls"
            }
        }
            
            stage('build') {
            steps {
                echo 'build'
                script{
                withCredentials([usernamePassword(credentialsId:'docker-hub-login',usernameVariable: 'USERNAME',passwordVariable:'PASSWORD')]){
                 sh '''
                    docker login -u ${USERNAME}  -p ${PASSWORD}
                    docker build -t merit237/backhousimagejenkins:v${BUILD_NUMBER} .
                    docker push merit237/backhousimagejenkins:v${BUILD_NUMBER}
                    '''
                    
                    
                        }
                    }
            
                }
            }
            stage('deploy') {
            steps {
                echo 'deploy'
                  script{
                withCredentials([file(credentialsId:'jenkins-slvae-kubeconfig',variable: 'KUBECONFIG')]){
                 sh '''
                     kubectl apply -f Deployment --kubeconfig ${KUBECONFIG}                    '''
                    
                    
                        }
                    }
                }              
           }
      }
 }

