node {
    def app
	
	environment {
    REPOSITORY = "shiniga666"
    APP_NAME = "dimas-java"
	SERVICE_NAME = "dimas-java"
    BUILD_NUMBER = "latest"
    IMAGE_TAG = "${env.REPOSITORY}/${env.APP_NAME}:${env.BUILD_NUMBER}"
    }

   stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }
	
	stage('build java apps') {
        /* build using maven */
	  steps {
		sh """
		cd docker/
		mvn clean package
		"""
        echo "build jar file done"
	  }
    }
    stage('Build image and push image') {
		container('docker') {
        withCredentials([[$class: 'UsernamePasswordMultiBinding',
          credentialsId: 'dockerhub',
          usernameVariable: 'shiniga666',
          passwordVariable: 'some strong password']]) {
          sh """
		    cd docker/
            docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}
            docker build -t ${REPOSITORY}/${APP_NAME}:${BUILD_NUMBER} .
			docker push ${REPOSITORY}/${APP_NAME}:${BUILD_NUMBER}
            """
        }
    }

   	stage('Deploy Dev') {
      // Developer Branches
      when {
        not { branch 'master' }
        not { branch 'stage' }
      }
      steps {
        container('kubectl') {
          // update service with new image
		  sh "kubectl set image deployment/${SERVICE_NAME} ${SERVICE_NAME}=${REPOSITORY}/${APP_NAME}:${BUILD_NUMBER}"
          echo "To access your environment go to dev website, since this apps already setup with nginx webserver"
        }
      }
	
}
