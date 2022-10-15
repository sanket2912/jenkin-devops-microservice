pipeline {
	agent { docker {image 'node:13.8'}}
	stages{
		stage('Build'){
			steps{
				sh 'node --version'
				echo "Build"
			}
		}
		stage('Test'){
			steps{
				echo "Test"
			}
		}
		stage('Integration Test') {
			steps{
				echo "Integration Test"
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