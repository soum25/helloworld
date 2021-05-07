pipeline  {
	environment {
		IMAGE_NAME = "helloworld"
		IMAGE_TAG = "latest"
		IMAGE_REPO = "soum25"
	}
	agent none
	stages {
		stage('Build image') {
			agent any
			steps {
				script {
					sh 'docker build -t $IMAGE_REPO/$IMAGE_NAME:$IMAGE_TAG .'
				}
			}
		}
		stage('Run container based on builded image') {
			agent any
			steps {
				script {
					sh '''
					docker run --name $IMAGE_NAME -d -p 80:80 $IMAGE_REPO/$IMAGE_NAME:$IMAGE_TAG
					sleep 5
					'''
				}
			}
		}
		stage('Test container') {
			agent any
			steps {
				script {
					sh '''
					var=$(curl http://172.17.0.1:80)
					if [ "$var" = 'Hello universe!' ]; then exit 0; else exit 1; fi
					'''
				}
			}
		}		
		stage('Clean Container') {
			agent any
			steps {
				script {
					sh '''
					  docker rm -vf ${IMAGE_NAME}
					'''
                                }
                        }
                }
		stage('Push image on dockerhub') {
                      agent any 
                      environment {
                         DOCKERHUB_LOGIN = credentials('dockerhub-soumahoro')
                
                       }

                         steps {
                                 script {
                                         sh '''
					docker login --username ${DOCKERHUB_LOGIN_USR} --password ${DOCKERHUB_LOGIN_PSW}
					docker push ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG}
                                         '''
                                 }
                         }
                }
        }
}
