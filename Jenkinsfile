pipeline {

    agent any


    stages {
        
        stage('Cleanup Workspace') {
            steps {
                sh """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage('Code Checkout') {
            steps {
                 sh """
                      echo "Code Checkout"
                    """
            }
        }

        stage(' Unit Testing') {
            steps {
                sh """
                echo "Running Unit Tests"
                """
            }
        }

        stage('Code Analysis') {
            steps {
				parameters {
            booleanParam(name: 'BUILD_DOCKER_IMAGE', defaultValue: true, description: 'Uncheck to skip creation of Docker image.')
			}
                sh """
                echo "Running Code Analysis ${BUILD_DOCKER_IMAGE}"
                """
            }
        }
		
		stage('Approve Publish version to  sparta backend build') {
            when {
                beforeAgent true
                branch 'master'
            }
            steps {
                timeout(time: 30, unit: 'MINUTES') {
                    script {
                        // Show the select input modal
                       def INPUT_PARAMS = input message: 'Please Provide Parameters', ok: 'Next',
                                        parameters: [
                                        choice(name: 'ENVIRONMENT_HEPROD', choices: ['hepro'], description: 'Please select the Environment'),
                                        choice(name: 'ENVIRONMENT_PROD', choices: ['pro'], description: 'Please select the Environment')]
                        env.ENVIRONMENT_HEPROD = INPUT_PARAMS.ENVIRONMENT_HEPROD
                        env.ENVIRONMENT_PROD = INPUT_PARAMS.ENVIRONMENT_PROD
                    }
                }
            }
        }

        stage('Build Deploy Code HE PROD') {
			when {
                expression {
                    return ENVIRONMENT_HEPROD == "hepro"
                }
            }
            steps {
                sh """
                echo "Building Artifact "
                """

                sh """
                echo "Deploying Code"
                """
            }
        }
		
		stage('Build Deploy Code - PROD') {
			when {
                expression {
                    return ENVIRONMENT_PROD == "pro"
                }
            }
            steps {
                sh """
                echo "Building Artifact"
                """

                sh """
                echo "Deploying Code"
                """
            }
        }

    }   
}
