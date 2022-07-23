pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('docker-hub')
	}

	stages {

		stage('Docker Build And Tag') {

			steps {
				sh 'docker build -t ayushsarin1991/my_testing:latest .'
				sh 'docker tag my_testing ayushsarin1991/my_testing:latest'
				sh 'docker tag my_testing ayushsarin1991/my_testing:$BUILD_NUMBER'
			}
		}

		stage('Docker Hub Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push to Docker Hub') {

			steps {
				sh 'docker push ayushsarin1991/my_testing:latest'
				sh 'docker push ayushsarin1991/my_testing:$BUILD_NUMBER'
			}
		}
        stage('Cleaning up') { 

            steps { 
                sh 'docker rmi ayushsarin1991/my_testing:$BUILD_NUMBER'
				sh 'docker rmi ayushsarin1991/my_testing:latest'
            }
        } 
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
