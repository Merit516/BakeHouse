pipeline {
    agent {label 'jenkins-slave'}
    parameters{
            choice(name: 'ENV' ,choices:['dev','test','preprod','relese'])
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
                    if(params.ENV == "release" ){
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
                 if(params.ENV == "dev" || params.ENV == "test" || params.ENV == "preprod") { 
                                withCredentials([file(credentialsId:'kubeconfig-slave-id',variable: 'KUBECONFIG')]){
                                 sh '''
                                     export BUILD_NUMBER=$(cat ../build.txt)
                                     mv  Deployment/deploy.yaml  Deployment/deploy.yaml.tmp
                                     cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                     rm -f Deployment/deploy.yaml.tmp
                                     kubectl apply -f Deployment --kubeconfig ${KUBECONFIG} -n ${ENV}

                                     '''


                                }
                           }
                    }
                }              
           }
      }
 }
