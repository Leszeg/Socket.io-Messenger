pipeline{
	agent any
	tools {
		nodejs "node"
	}
	stages {
		stage('Build') {
			steps {
				echo 'Building'
				sh 'git pull origin master'
				sh 'npm install'
			}
			post{
				always{
					echo 'Finished'
				}
				failure{
					statusAllert('Build', 'Failure')
				}
				success{
					statusAllert('Build', 'Success')
				}
			}
		}
		stage('Test') {
			steps {
				echo 'Testing'
				sh 'npm run test'
			}
			post{
				always{
					echo 'Finished'
				}
				failure{
					statusAllert('Test', 'Failure')
				}
				success{
					statusAllert('Test', 'Success')
				}
			}
		}
		stage('Deploy') {
            steps {
                echo 'Deploy'
                sh 'docker build -t socketio_deploy -f Dockerfile_socketio_deploy .'
            }
            post {
				always{
					echo 'Finished'
				}
				failure{
					statusAllert('Deploy', 'Failure')
				}
				success{
					statusAllert('Deploy', 'Success')
				}
            }
        }
	}
}

def statusAllert(stage, status) {
	echo status
	emailext attachLog: true,
		body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
		to: 'olekn41@gmail.com',
		subject: stage + status
}
