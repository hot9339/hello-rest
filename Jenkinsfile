node {
	stage('1. Source Code Pull'){
		git branch:'master', url:'https://github.com/hot9339/hello-rest.git'
	}
	stage('2. Source Code Package'){
		sh 'mvn clean package -DskipTests=true'
	}
	stage('3. Source Code JunitTest'){
		sh 'mvn surefire-report:report'
	}            
	stage('4. Container Image Build'){
		sh 'docker build -t hot9339/hello-rest .'
	}  	
	stage('5. Container Image Push'){
		sh 'docker login -u hot9339 --password-stdin < ~/docker-pass'
		sh 'docker push hot9339/hello-rest'
	}  	
	stage('6. Container Running'){
		def containerID = sh (script: 'docker ps -aq -f name=hello-rest', returnStdout: true)
		if (containerID != '') {
			sh 'docker stop ' + containerID
		}
		sh 'docker run -d -p 80:80 --rm --name hello-rest hot9339/hello-rest'
	}
}
