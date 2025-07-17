//EKS Project
pipeline {
	agent any
	stages {
	
		stage('Checkout') {
			steps {
				echo 'Git Checkout'
				git branch : 'main', url : 'https://github.com/jeenapandit/aer-capstone-ekscluster.git'
		
			}
		}
	
		stage('Create EKS Cluster') {
			steps {
				script {
					withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWSID', 
					accessKeyVariable: 'AWS_ACCESS_KEY_ID',  secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
					{
					// Check AWS credentials are set correctly
					sh 'aws sts get-caller-identity'
					sh 'eksctl version'
					sh 'ls -lrt'
					echo 'Creating EKS Cluster'
					sh 'eksctl create cluster -f cluster.yaml'
					sh 'kubectl cluster-info'
					}
				}
			}
		}
	
		stage('Deploy') {
            steps {
                script {
					echo 'Deploying Services'
					withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWSID', 
					accessKeyVariable: 'AWS_ACCESS_KEY_ID',  secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
					{
						sh 'kubectl apply -f env_namespace.yaml'
						sh 'kubectl get namespaces'
						sh 'kubectl apply -f internal-deployment.yaml --namespace=main'
						sh 'kubectl apply -f external-deployment.yaml --namespace=main'
						sh 'kubectl apply -f external-service.yaml --namespace=main'
						sh 'kubectl apply -f internal-service.yaml --namespace=main'
                    }
                }
			}
        }

        stage('Test') {
			agent any
			steps {
				echo 'Testing Deployments'
				withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWSID', 
				accessKeyVariable: 'AWS_ACCESS_KEY_ID',  secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
				{
					sh 'kubectl get deploy -n main'
					sh 'kubectl get pods -n main'
					sh 'kubectl get services -n main'
					echo 'Loadbalancer DNS'
				}
			}
		}	

		
  }
}
