pipeline {
	agent any

    environment {
		GIT_COMMIT_HASH = ''
    }

    stages {
		stage("Checkout") {
			steps {
				script {
					checkout scm
                    env.GIT_COMMIT_HASH = sh(script: "git log -n 1 --pretty=format:%H", returnStdout: true).trim()
                }
            }
        }

        stage('Install Dependencies') {
			steps {
				sh 'pip install -r requirements.txt'
            }
        }

        // stage('Run Tests') {
        //     steps {
        //         sh 'pytest'
        //     }
        // }

        stage('Build & Run Docker') {
			steps {
				sh 'docker build -t my-python-app .'
                sh 'docker run -d -p 8000:8000 my-python-app'
            }
        }

        stage("Push Docker image") {
			steps {
				withCredentials([usernamePassword(credentialsId: 'mchekini', passwordVariable: 'password', usernameVariable: 'username')]) {
					script {
						sh "sudo docker login -u $username -p $password"
                        sh "sudo docker push mchekini/scpi-invest-plus-api:$GIT_COMMIT_HASH"
                        sh "sudo docker rmi mchekini/scpi-invest-plus-api:$GIT_COMMIT_HASH"
                    }
                }
            }
        }
    }
}
