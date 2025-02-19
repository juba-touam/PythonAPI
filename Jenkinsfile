node {
	def GIT_COMMIT_HASH = ""

	stage("Checkout"){
		checkout scm
        GIT_COMMIT_HASH = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)
    }

    stage('Build Docker Image') {
			sh "sudodocker build -t mchekini/my-python-app:$GIT_COMMIT_HASH ."
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

