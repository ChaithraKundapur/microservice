pipeline {
  agent any

	tools {
		nodejs "NodeJS"
	}
	
	environment {
		
		        PROJECT_ID = "tech-rnd-project"
				CLUSTER_NAME = "network18-cluster"
				LOCATION = "us-central1-a"
				CREDENTIALS_ID = "kubernetes"			
	}
	
    stages {
	    stage('Scm Checkout') {
		    steps {
			    	checkout scm
		    }
	    }
	    stage('build') {
              steps {
                  echo 'building the software'
                  sh 'npm install'
              }
          }

	    
	    stage('Build Docker Image') {
		    steps {
			    sh 'whoami'
			    sh 'sudo chmod 777 /var/run/docker.sock'
			    
			    sh ' sudo apt update'
 			    sh 'sudo apt install software-properties-common -y'
			    

			    
				    
				sh 'sudo add-apt-repository ppa:cncf-buildpacks/pack-cli'
			    
 				 sh 'sudo  apt-get update'
 				  sh 'sudo apt-get install pack-cli'
			   
				  sh 'pack build todo -t gcr.io/tech-rnd-project/todo --builder paketobuildpacks/builder:full'
			    
		    }
	    }
	    
	    stage("Push Docker Image") {
		    steps {
			    script {
				    echo "Push Docker Image"
				        sh 'gcloud auth configure-docker'
				        sh "sudo docker push gcr.io/tech-rnd-project/todo"
				    
					sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'

					sh "chmod +x kubectl"

					sh "sudo mv kubectl \$(which kubectl)"

				    
			    }
		    }
	    }
	    
	    stage('Deploy to K8s') {
		    steps{
			    echo "Deployment started ..."
			    sh 'ls -ltr'
			    sh 'pwd'
				
				echo "Start deployment of deployment.yaml"
				step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'k8', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
			    	echo "Deployment Finished ..."
			    sh '''
			    '''
			    
		    }
	    }
    }
}