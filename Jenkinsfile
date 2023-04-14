pipeline {
    agent any

    
    stages {
        stage('Login, Build and Push'){
            steps {
                script{
                    //sign into docker, build image, and push image to dockerhub
                    withDockerRegistry(credentialsId: 'Docker') {
                        docker.build('harleyguy/flaskapp').push('latest')
                    }
                }
            }
        }
        stage('AWS Commands'){
            steps {
                
                    // sign into AWS
                    
                        sh 'aws sts get-caller-identity'
                    
            }
        }
        stage('Kubernetes login'){
            steps{

                  
                    
                        sh 'aws eks update-kubeconfig --region us-east-1 --name hd'


            }
        }
        stage('Create Namespace'){
            steps {
                script {
                    try {
                            sh 'kubectl apply -f manifest.yaml'
                            sh 'kubectl rollout restart deployment flask-deployment -n hd-namespace'
                        } catch (Exception e) {
                            echo 'Exception occured: ' + e.toString()
                            echo 'Handled the Exception!'
                        }
                    }
                }
            }
        }
    }