pipeline {
    agent {
        node {
            label 'linux'
        }
    }
    tools {
        maven 'maven'
    }
    options {
        buildDiscarder logRotator(
                daysToKeepStr: '15',
                numToKeepStr: '10'
            )
    }
    environment {
        APP_NAME = "DCUBE_APP"
        APP_ENV  = "DEV"
    }
    
    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace for ${APP_NAME}"
                """
            }
        }
        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/master']], 
                    userRemoteConfigs: [[url: 'https://github.com/erhan-ekici/jenmvn.git']]
                ])
            }
        }
        stage('Code Build') {
            steps {
                 sh 'mvn install -Dmaven.test.skip=true'
            }
        }

        
	stage ('***** Paralel Stage *****'){
		parallel {
			stage ('P Stage 1 - Executing Shell'){
				steps {
					sh 'echo "Shell Command Execution Step"'
				}
			}
			stage('P Stage 2 - Priting All Global Variables') {
            			steps {
                			sh """
                			env
                			"""
            			}
        		}
			stage ('P Stage 3 - Executing Shell'){
				steps {
					sh 'echo "Shell Command Execution Step"'
				}
			}
		}
	}
    }
}
