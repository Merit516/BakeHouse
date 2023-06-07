pipeline {
    agent {label 'jenkins-slave'}
    environment {
    ENV = [
        'params1': 'dev',
        
        'params2': 'test',
        'params3': 'release',
        
    ]
}
    parameters{
            choice(name: 'ENV' ,choices:['dev','test','release'])
    }
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
                    if('params3'){
                            withCredentials([usernamePassword(credentialsId:'docker-hub-login',usernameVariable: 'USERNAME',passwordVariable:'PASSWORD')]){
                             sh '''
                                docker login -u ${USERNAME}  -p ${PASSWORD}
                                docker build -t merit237/backhousimagejenkins:v${BUILD_NUMBER} .
                                docker push merit237/backhousimagejenkins:v${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../build.txt

                                '''


                                    }
                             }
                             else {
                        echo "user choosed ${ENV}"
                    }
                       }
                  }
            }
            stage('deploy') {
            steps {
                echo 'deploy'
                  script{
                 if('params1' ||'params2' ) { 
                                withCredentials([file(credentialsId:'kubeconfig-slave-id',variable: 'KUBECONFIG')]){
                                 sh '''
                                     export BUILD_NUMBER=$(cat ../build.txt)
                                     mv  multi-branch/deploy.yaml  multi-branch/deploy.yaml.tmp
                                     cat multi-branch/deploy.yaml.tmp | envsubst > multi-branch/deploy.yaml
                                     rm -f multi-branch/deploy.yaml.tmp
                                     helm install multi-branch ./multi-branch --values values.yaml--kubeconfig $KUBECONFIG -n $ENV

                                     '''


                                }
                           }
                    }
                }              
           }
      }
 }
