pipeline {
	agent none
	environment {
		NODE_VER = '8.1.0'
	}
	
	/*options {
		skipDefaultCheckout() //does not pull code everytime, you have to manually make a pull request
		timeout(time: 1, unit: 'DAYS') //if there is input that is ignored for 1 day, it is aborted
	}*/
	
	/*post {
		//success {}
		//failure {}
		//always {}
		//unstable {}
		//aborted {}
	}*/
	stages {
		stage('Beginning') { agent any
			environment {
				DEPLOY_VERSION = 'stage'
			}
			steps {
				echo 'Hello world'
				sh 'echo $NODE_VER'
				echo "${env.NEW_VAR}"
			}
		}
		
		stage('Who Am I?') { agent any
			environment {
				DEPLOY_VERSION = 'prod'
			}
			steps {
				echo "${env.DEPLOY_VERSION}"
				sh 'host -t TXT pgp.michaelholley.us | awk -F \'"\' \'{print $2}\''
			}
		}
		
		stage('Deploy to stage?') { agent none
			when {
				anyOf {
					branch 'stage'
					environment name: 'NODE_VER', value: '8.1.0'
				}
			}
			steps {
				input 'Deploy to stage?'
			}
		}
		
		stage('Parallel') {
			failFast true
			parallel {
				stage('Build 1') { agent any
					steps {
						echo "It's ME!"
					}
				}
				   
				stage('Build 2') { agent any
					steps {
						echo "Not It's ME!"
					}
				}
			}
		}
	}
}
