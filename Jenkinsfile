pipeline {
	agent any

    environment {
		GIT_COMMIT_HASH = ''
    }

    stage("Checkout") {
		checkout scm
        env.GIT_COMMIT_HASH = sh(script: "git log -n 1 --pretty=format:%H", returnStdout: true).trim()
    }

    stage('Build Docker Image') {
		sh 'docker build -t my-python-app .'
    }

    stage('Run Tests Inside Docker') {
		sh 'docker run --rm my-python-app pytest'
    }

    stage("Push Docker image") {
		withCredentials([usernamePassword(credentialsId: 'mchekini', passwordVariable: 'password', usernameVariable: 'username')]) {
			sh "docker login -u $username -p $password"
            sh "docker tag my-python-app mchekini/scpi-invest-plus-api:$GIT_COMMIT_HASH"
            sh "docker push mchekini/scpi-invest-plus-api:$GIT_COMMIT_HASH"
            sh "docker rmi mchekini/scpi-invest-plus-api:$GIT_COMMIT_HASH"
        }
    }
}
