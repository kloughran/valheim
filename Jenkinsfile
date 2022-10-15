pipeline {
    agent any 
	
	stages {
		stage('remove old') { 
			steps {
				sh 'docker stop valheim || true'
				sh 'docker rm valheim || true'
			}
		}
		stage('update') {
			steps {
				withCredentials([usernamePassword(credentialsId: 'dockercreds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
				  sh 'docker login --username $USERNAME --password $PASSWORD'
				}
				sh 'docker pull lloesche/valheim-server'
			}
		}
		stage('deploy') {
			steps {
				withCredentials([usernamePassword(credentialsId: 'valheim', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
					sh 'docker run -d --name valheim -v /media/valheim-data:/config --restart unless-stopped -p 2456-2458:2456-2458/udp -e SERVER_NAME="$USERNAME" -e SERVER_PASS="$PASSWORD" -e WORLD_NAME="$USERNAME" -e SERVER_PUBLIC=0 -e UPDATE_INTERVAL=2592000 -e BACKUPS=false lloesche/valheim-server'
				}
			}
		}
	}
}