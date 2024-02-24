pipeline {
    agent { label 'Slave_Node1' }
    
   tools {
        // Define Maven tool installation
        maven 'maven-3.6.3'
    }
    environment {	
		DOCKERHUB_CREDENTIALS=credentials('dockerhub_credentials')
	} 

    stages {
        stage('SCM Checkout') {
            steps {
		echo 'Perform SCM Checkout'
           	git 'https://github.com/dlfarande/star-agile-insurance-project.git'
            }
        }
        stage('Maven Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
       stage("Docker build"){
            steps {
				sh 'docker version'
				sh "docker build -t dlfarande/dlf-insurance-eta-app:${BUILD_NUMBER} ."
				sh 'docker image list'
				sh "docker tag dlfarande/dlf-insurance-eta-app:${BUILD_NUMBER} dlfarande/dlf-insurance-eta-app:latest"
            }
        }
		stage('Login2DockerHub') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
		stage('Push2DockerHub') {

			steps {
				sh "docker push dlfarande/dlf-insurance-eta-app:latest"
			}
		}
    }
}
