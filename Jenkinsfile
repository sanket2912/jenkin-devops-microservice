pipeline {
	// agent { docker {image 'maven'}}
	// agent { docker {image 'node:13.8'}}
	agent any
	environment{
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages{
		stage('Checkout'){
			steps{
				sh 'mvn --version'
				sh 'docker --version'
				echo "Build_PATH = $PATH"
				echo "BUILD_NUMBER  = $env.BUILD_NUMBER"
				echo "BUILD_ID = $env.BUILD_ID"
				echo "JOB_NAME = $env.JOB_NAME"
				echo "BUILD_TAG = $env.BUILD_TAG"
				echo "BUILD_URL = $env.BUILD_URL"

			}
		}
		stage('Compile'){
			steps{
				sh " mvn clean compile"
			}
		}
		stage('Test'){
			steps{
				sh "mvn test"//is to run your unit tests.
			}
		}
		stage('Integration Test') {
			steps{
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}

		stage('Build Docker Image'){
			steps{
				script{
					dockerImage = docker.build("sanku291297/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}
		stage('Push Docker image'){
				steps{
					script{
						docker.withRegistry('','dockerhub'){
							dockerImage.push();
							dockerImage.push('latest');
						}
					}
				}
		}
		stage('Package'){
			steps{
				sh "mvn package -DskipTests"
			}
		}
	} 
	post{
		always{
			echo 'Im awesome, I run always'
		}
		success{
			echo 'I run when you are successfull'
		}
		failure{
			echo 'I run when you fail'
		}
	}
}