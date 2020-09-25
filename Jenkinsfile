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
                sh """
                echo "Running Code Analysis"
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
                                        parameters: {
											choice(name: 'CHOICE',
											choices: ['hepro, pro'], 
											description: 'Please select the Environment')
										}
                    }
                }
            }
        }

        stage('Build Deploy Code HE PROD') {
			when {
                expression {
                    return "${INPUT_PARAMS.CHOICE}" == "hepro"
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
		
		stage('Build Deploy Code - PROD') {
			when {
                expression {
                    return "${INPUT_PARAMS.CHOICE}"== "pro"
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
