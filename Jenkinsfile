pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build Docker') {
          steps {
            // sh 'echo "building the repo"'
            // build the docker image from the source code using the BUILD_ID parameter in image name
            sh "sudo docker build -t flask-app ."
          }
        }
      }
    }

    stage('Test') {
      steps {
        sh 'python3 test_app.py'
        input(id: "Deploy Gate", message: "Deploy ${params.project_name}?", ok: 'Deploy')
      }
    }

    stage('run docker container')
    {
      steps {
        // echo "deploying the application"
        sh "sudo docker run -p 8000:8000 --name flask-app -d flask-app "
      }
    }

  }

  post {
        always {
            echo 'The pipeline completed'
            junit allowEmptyResults: true, testResults:'**/test_reports/*.xml'
        }
        success {
            
            sh "sudo nohup python3 app.py > log.txt 2>&1 &"
            echo "Flask Application Up and running!!"
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early…')
        }
      }
}
