pipeline {
  agent any
   stages{

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://126791609773.dkr.ecr.us-east-2.amazonaws.com', 'ecr:us-east-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	   
	stage('Kubernetes Deployment of ASG Bugg Web Application') {
	   steps {
		   echo "before Kubelogin"
	      withKubeConfig([credentialsId: 'kubelogin']) {
		  echo "after kubelogin"
		  sh('kubectl delete all --all -n dev')
		  echo "after deleting the namespace"
		  sh ('kubectl apply -f deployment.yaml --namespace=dev')
		  echo "after deploying kubernetes"
		}
	      }
   	}

  }
}
