pipeline {
    agent any
    stages {
    // Building Docker images
        stage('ecr push') {
            steps {     
		    sh 'aws ecr get-login-password --region ap-south-1 | sudo docker login --username AWS --password-stdin 012333045906.dkr.ecr.ap-south-1.amazonaws.com'
	        sh 'sudo docker build -t jenkins-ecr:v${BUILD_NUMBER} -t jenkins-ecr:latest .'
	        sh 'sudo docker tag jenkins-ecr:v${BUILD_NUMBER} 012333045906.dkr.ecr.ap-south-1.amazonaws.com/jenkins-ecr:v${BUILD_NUMBER}'
		    sh 'sudo docker tag jenkins-ecr:v${BUILD_NUMBER} 012333045906.dkr.ecr.ap-south-1.amazonaws.com/jenkins-ecr:latest'
	        sh 'sudo docker push 012333045906.dkr.ecr.ap-south-1.amazonaws.com/jenkins-ecr:v${BUILD_NUMBER}'
		    sh 'sudo docker push 012333045906.dkr.ecr.ap-south-1.amazonaws.com/jenkins-ecr:latest'
            }
        }
   

       stage('K8S Deploy') {
        steps{   
            script {
                withKubeConfig([credentialsId: 'K8S', serverUrl: '']) {
                sh ('kubectl apply -f  eks-deploy-k8s.yaml')
                }
            }
        }
       }
    }
}
