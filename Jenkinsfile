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
	agent any
	// here our agent is a docker image
	// agent { docker { image 'maven:3.6.3' } }
	// agent { docker { image 'node:14.19.2' } }

	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}

	stages {
		stage('Checkout') {
			steps {
				sh 'mvn --version'
				sh 'docker --version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"
			}
		}
		stage('Compile') {
			steps {
				// this is also similar to npm install which install all your packages
				sh "mvn clean compile"
			}
		}
		stage('Package') {
			steps {
				// run the mnv package and skip the test
				sh "mvn package -DskipTests"
			}
		}

		stage('Test') {
			steps {
				// this is to run your unit test just similar to your npm test
				sh "mvn test"
				// echo "Test"
			}
		}

		stage('Build Docker Image') {
			steps {
				// here we will be building a docker image
				// docker build -t ugochukwu15/currency-exchange:$env.BUILD_TAG .
				// the above is a primitive way of building your docker
				// a more elaborate way is using a script block
				script {
					dockerImage = docker.build("ugochukwu15/currency-exchange:${env.BUILD_TAG}")
				}
			}
		}

		stage('Push Docker Image') {
			steps {
				// here we will be pushing a docker image
				script {
					// attach your docker hub credential to the push below using 'withRegistry'
					docker.withRegistry('', 'dockerhub'){
						dockerImage.push()
						dockerImage.push('latest')
					}
				}
			}
		}

		stage('Integration Test') {
			// here you can run your integration test by running you node integration test script
			steps {
				sh "mvn failsafe:integration-test failsafe:verify"
				// echo "Integration Test"
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
