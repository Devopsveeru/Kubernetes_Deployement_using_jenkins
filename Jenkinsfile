pipeline {
    agent any
    stages {
    // Building Docker images
        stage('ecr push') {
            steps {     
		    sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 012333045906.dkr.ecr.ap-south-1.amazonaws.com'
	            sh 'docker build -t jenkins-ecr:v${BUILD_NUMBER} -t jenkins-ecr:latest .'
	            sh 'docker tag jenkins-ecr:v${BUILD_NUMBER} 012333045906.dkr.ecr.ap-south-1.amazonaws.com/jenkins-ecr:v${BUILD_NUMBER}'
		    sh 'docker tag jenkins-ecr:v${BUILD_NUMBER} 012333045906.dkr.ecr.ap-south-1.amazonaws.com/jenkins-ecr:latest'
	            sh 'docker push 012333045906.dkr.ecr.ap-south-1.amazonaws.com/jenkins-ecr:v${BUILD_NUMBER}'
		    sh 'docker push 012333045906.dkr.ecr.ap-south-1.amazonaws.com/jenkins-ecr:latest'
            }
        }
   

       stage('K8S Deploy') {
        steps{   
            script {
                withKubeConfig([credentialsId: 'k8s1', serverUrl: '']) {
                sh ('kubectl apply -f  eks-deploy-k8s.yaml')
                }
            }
        }
       }
    }
}
