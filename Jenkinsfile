node("ci-node") {
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

        stage('Build Docker Image') {
			steps {
				sh 'docker build -t my-python-app .'
            }
        }

        //stage('Run Tests Inside Docker') {
		//	steps {
		//		sh 'docker run --rm my-python-app pytest' // Exécute les tests avec pytest à l'intérieur du conteneur
        //    }
        //}

        stage("Push Docker Image") {
			steps {
				withCredentials([usernamePassword(credentialsId: 'mchekini', passwordVariable: 'password', usernameVariable: 'username')]) {
					sh "sudo docker login -u $username -p $password"
                    sh "sudo docker push mchekini/bibliotheque-api:$GIT_COMMIT_HASH"
                    sh "sudo docker rmi mchekini/bibliotheque-api:$GIT_COMMIT_HASH"
                }
            }
        }
    }
}
