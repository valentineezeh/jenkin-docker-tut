// scrited approach the stage('build'){} are not mandatory
// stage block are optional
// node {
// 	echo "Build"
// 	echo "Test"
// 	echo "Integration Test"
// }

// Declarative pipeline
// agent is similar to node but it gives you a lot of flexibility
pipeline {
	// use any of the agent
	// agent any
	// here our agent is a docker image
	// agent { docker { image 'maven:3.6.3' } }
	agent { docker { image 'node:14.19.2' } }

	stages {
		stage('Build') {
			steps {
				// sh 'mvn --version'
				sh 'node --version'
				echo "Build"
			}
		}
		stage('Test') {
			steps {
				echo "Test"
			}
		}
		stage('Integration Test') {
			steps {
				echo "Integration Test"
			}
		}
	}
	post {
		always {
			echo 'I am awesome. I run always'
		}
		success {
			// do something after a success
			echo 'I run when you are successful'
		}
		failure {
			// do something after a failure
			echo 'I run when you fails'
		}
		// changed {
		// 	// do something when the status of the build changes
		// 	echo 'i run when the status of the build changes'
		// }
	}
}
