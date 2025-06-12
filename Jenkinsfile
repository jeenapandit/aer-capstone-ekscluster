//EKS Project development
pipeline {
	agent any
	stages {
	
		stage('Checkout') {
			steps {
				echo 'Git Checkout'
				git branch : 'development', url : 'https://github.com/jeenapandit/aer-capstone-ekscluster.git'
		
			}
		}
	
		stage('Check EKS') {
			steps {
				script {
					withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-profile-id', 
					accessKeyVariable: 'AWS_ACCESS_KEY_ID',  secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
					{
					// Check AWS credentials are set correctly
					sh 'aws sts get-caller-identity'
					sh 'eksctl version'
					sh 'ls -lrt'
					}
				}
			}
		}
	
		stage('Deploy') {
            steps {
                script {
					echo 'Deploying Services'
					withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-profile-id', 
					accessKeyVariable: 'AWS_ACCESS_KEY_ID',  secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
					{
						sh 'kubectl apply -f env_namespace.yaml'
						sh 'kubectl get namespaces'
						sh 'kubectl apply -f internal-deployment.yaml --namespace=development'
						sh 'kubectl apply -f external-deployment.yaml --namespace=development'
						sh 'kubectl apply -f external-service.yaml --namespace=development'
						sh 'kubectl apply -f internal-service.yaml --namespace=development'
                    }
                }
			}
        }

        stage('Test') {
			agent any
			steps {
				echo 'Testing Deployments'
				withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-profile-id', 
				accessKeyVariable: 'AWS_ACCESS_KEY_ID',  secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
				{
					sh 'kubectl get deploy -n development'
					sh 'kubectl get pods -n development'
					sh 'kubectl get services -n development'
				}
			}
		}	

		
  }
}
