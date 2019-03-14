node {
 
    stage 'Checkout'
        checkout scm
        
    stage 'Packaging Test'
		sh "mvn clean install -e -X"
		
		sh "mvn clean package"
		
		
    stage 'Build Test'
        sh "docker build -t jenkins-test-docker:B${BUILD_NUMBER} ."
  
    stage 'Run Test'
        containerID = sh (
            script: "docker run -d jenkins-test-docker:B${BUILD_NUMBER}", 
        returnStdout: true
        ).trim()
        echo "Container ID is ==> ${containerID}"
        sh "docker stop ${containerID}"
        sh "docker rm ${containerID}"

    stage 'Push'
		sh "docker image tag jenkins-test-docker:B${BUILD_NUMBER} junyoungman/jenkins-test-docker:B${BUILD_NUMBER}"
		
		sh "docker push junyoungman/jenkins-test-docker:B${BUILD_NUMBER}"
}