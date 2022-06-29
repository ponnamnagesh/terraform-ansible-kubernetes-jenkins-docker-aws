pipeline{

    agent any
    
    stages {
        stage('SCM Checkout'){
            steps {
                // remove folder if already exist
                sh 'rm -rf terraform-ansible-kubernetes-jenkins-docker-aws'

                // clone the repository from github
                sh 'git clone https://github.com/ponnamnagesh/terraform-ansible-kubernetes-jenkins-docker-aws.git'
            }
        }
        

        stage ('Building New Docker Image') {
            steps {
                script {
                    dir('./docker') {
                        //  Building new image
                        //sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
                        sh 'docker build -t claimvisionecr .'
                        //sh 'docker tag latest:$BUILD_NUMBER'
                        //sh 'docker tag latest[:$BUILD_NUMBER]'
                        sh 'docker tag claimvisionecr:latest 004738182300.dkr.ecr.us-east-2.amazonaws.com/claimvisionecr:latest'
                        echo "Image successfully built"
                    }
                }
            }
        }

        stage('Updating Image On ECR'){
            steps {
                sh 'sudo su -'
                sh 'docker push 004738182300.dkr.ecr.us-east-2.amazonaws.com/claimvisionecr:latest'
                echo "Image has been updated on ecr"
                
                }
            }
        

        stage('Starting Kubernetes') {
            steps {
                script{
                    
                    sh 'minikube start'
                    echo 'kubernetes started successfully'
                    
                }
            }
        }
        
        stage('Terraform init') {
            steps {
                script{
                   sh 'terraform init'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script{
                     sh 'terraform apply -auto-approve'
                }
            }
        }

        stage('Pods Info') {
            steps {
                script{
                    
                    sh 'kubectl get po -A'
                    
                }
            }
        }

        stage('Deployment Info') {
            steps {
                script{
                    
                    sh 'kubectl get deployment'
                    
                }
            }
        }

        stage('Cluster Info') {
            steps {
                script{
                    
                    sh 'kubectl get all'
                    
                }
            }
        }
        


    }
}
