node {
 
    stage 'Checkout'
        checkout scm
        
    stage 'Packaging Test'
		sh "mvn clean install -e -X"
		
		sh "mvn clean package"
		
		
    stage 'Build Test'
        sh "docker build -t ${DOCKER_IMAGE}:${RELEASE_VERSION}${BUILD_NUMBER} ."
  
    stage 'Run Test'
        containerID = sh (
            script: "docker run -d ${DOCKER_IMAGE}:${RELEASE_VERSION}${BUILD_NUMBER}", 
        returnStdout: true
        ).trim()
        echo "Container ID is ==> ${containerID}"
        sh "docker stop ${containerID}"
        sh "docker rm ${containerID}"

    stage 'Push'
		sh "docker image tag ${DOCKER_IMAGE}:${RELEASE_VERSION}${BUILD_NUMBER} ${DOCKER_USERNAME}/${DOCKER_IMAGE}:${RELEASE_VERSION}${BUILD_NUMBER}"
		
		sh "docker push ${DOCKER_USERNAME}/${DOCKER_IMAGE}:${RELEASE_VERSION}${BUILD_NUMBER}"
}