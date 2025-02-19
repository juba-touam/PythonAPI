node {
	def GIT_COMMIT_HASH = ""

	stage("Checkout"){
		checkout scm
        GIT_COMMIT_HASH = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)
    }

	stage('Install dependencies') {
		sh "sudo docker run --rm -v ${PWD}:/app -w /app python:3.9 pip install -r requirements.txt"
	}


    stage('Build Docker Image') {
			sh "sudo docker build -t mchekini/my-python-app:$GIT_COMMIT_HASH ."
        }
    //stage('Run Tests') {
	//		sh "sudo docker run --rm ${imageName} pytest"
    //    }

    stage("Push Docker image"){
		withCredentials([usernamePassword(credentialsId: 'mchekini', passwordVariable: 'password', usernameVariable: 'username')]) {
			sh "sudo docker login -u $username -p $password"
            sh "sudo docker push mchekini/my-python-app:$GIT_COMMIT_HASH"
            sh "sudo docker rmi mchekini/my-python-app:$GIT_COMMIT_HASH"
        }
    }

}

