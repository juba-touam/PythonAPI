node {
	agent any

	environment {
		GIT_COMMIT_HASH = ''
	}

	stage("Checkout") {
		checkout scm
        env.GIT_COMMIT_HASH = sh(script: "git log -n 1 --pretty=format:%H", returnStdout: true).trim()
    }

    stage('Install Dependencies') {
		sh 'pip install -r requirements.txt'
    }

    // stage('Run Tests') {
    //     sh 'pytest'
    // }

    stage('Build & Run Docker') {
		sh 'docker build -t my-python-app .'
        sh 'docker run -d -p 8000:8000 my-python-app'
	}

    stage("Push Docker image"){
		withCredentials([usernamePassword(credentialsId: 'mchekini', passwordVariable: 'password', usernameVariable: 'username')]) {
			sh "sudo docker login -u $username -p $password"
            sh "sudo docker push mchekini/scpi-invest-plus-api:$GIT_COMMIT_HASH"
            sh "sudo docker rmi mchekini/scpi-invest-plus-api:$GIT_COMMIT_HASH"
        }
}
}