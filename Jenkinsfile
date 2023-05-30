pipeline {
    agent any

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
                sh ''' 
                      echo ${BUILD_NUMBER}
                '''
            }
            }
            stage('deploy') {
            steps {
                echo 'deploy'
            }
            
            
        }
    }
 }

