pipeline {
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        checkout scm
      }
    }
    
	  stage("Build image") {
		  steps {
			script {
				myapp = docker.build ("sample-nodejs:${env.BUILD_ID}")
			}			
		}
	}
		
	stage("Push image") {
		steps {
			script {
					docker.withRegistry('http://192.168.66.152:5000', 'localregistry') {
					myapp.push("latest")
					myapp.push("${env.BUILD_ID}")
				}
			}
		}
	}
	
    stage('Deploy App') {
      steps {
        script {
          sh "kubectl apply -f deployment.yaml"
          sh "kubectl -n sample-nodejs set image deployment/sample-nodejs sample-nodejs=192.168.66.152:5000/sample-nodejs:${env.BUILD_ID}"
        }
      }
    }
  }
}
