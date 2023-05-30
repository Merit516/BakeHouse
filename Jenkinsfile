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
                    docker login -u ${USERNAME}  -P ${PASSWORD}
                    docker build -t merit237/backhousimagejenkins:v1 .
                    docker push
                    '''
                    
                    
                        }
                    }
            
                }
            }
            stage('deploy') {
            steps {
                echo 'deploy'
            }
            
            
        }
    }
 }

