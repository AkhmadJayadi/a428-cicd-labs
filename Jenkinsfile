pipeline {
    agent {
        docker {
            image 'node:16-buster-slim' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install'
            }
        }
		stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
		stage('Manual Approval') {
	        input {
	            message 'Please select environment'
	            id 'envId'
	            ok 'Submit'
	            submitterParameter 'approverId'
	            parameters {
					choice choices: ['Prod', 'Pre-Prod'], name: 'envType'
	            }
	        }

	        steps {
	            echo "Deployment approved to ${envType} by ${approverId}."
	            }
	        }
		stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}