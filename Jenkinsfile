pipeline {
    agent any

    environment {
        // Define environment variables if needed (e.g., paths, versions, etc.)
        PYTHON_ENV = 'venv'
        REQUIREMENTS_FILE = 'requirements.txt'
        GIT_REPO_URL = 'https://github.com/SriteshSuranjan/brain-tumour-detection.git'
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout the latest code from your repository
                    echo "Checking out the code from Git repository"
                    git url: "${GIT_REPO_URL}"
                }
            }
        }

        stage('Set up Python Virtual Environment') {
            steps {
                script {
                    echo "Setting up Python virtual environment"
                    // Create virtual environment if it doesn't exist
                    sh 'python3 -m venv ${PYTHON_ENV}'
                    // Upgrade pip to the latest version
                    sh './${PYTHON_ENV}/bin/pip install --upgrade pip'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo "Installing dependencies from ${REQUIREMENTS_FILE}"
                    // Install the required dependencies from the requirements.txt file
                    sh './${PYTHON_ENV}/bin/pip install -r ${REQUIREMENTS_FILE}'
                }
            }
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    echo "Running static code analysis"
                    // Example: Running a linter (e.g., pylint or flake8)
                    sh './${PYTHON_ENV}/bin/pip install flake8'
                    sh './${PYTHON_ENV}/bin/flake8 .'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo "Running tests"
                    // Run unit tests or any other tests you have
                    // Example: pytest or unittest
                    sh './${PYTHON_ENV}/bin/python -m unittest discover'
                }
            }
        }

        stage('Build the Model') {
            steps {
                script {
                    echo "Training the machine learning model"
                    // Run your ML model training script
                    // Example: python train_model.py
                    sh './${PYTHON_ENV}/bin/python train_model.py'
                }
            }
        }

        stage('Evaluate Model') {
            steps {
                script {
                    echo "Evaluating the machine learning model"
                    // Run your model evaluation script
                    // Example: python evaluate_model.py
                    sh './${PYTHON_ENV}/bin/python evaluate_model.py'
                }
            }
        }

        stage('Package Model') {
            steps {
                script {
                    echo "Packaging the model for deployment"
                    // Example: Save or package the trained model (pickle, TensorFlow, etc.)
                    sh './${PYTHON_ENV}/bin/python package_model.py'
                }
            }
        }

        stage('Deploy Model') {
            steps {
                script {
                    echo "Deploying the model"
                    // Example: Deploy your model (could be an API, cloud service, etc.)
                    sh './${PYTHON_ENV}/bin/python deploy_model.py'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    echo "Cleaning up the workspace"
                    // Optionally clean up the workspace or virtual environment
                    sh 'rm -rf ${PYTHON_ENV}'
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }

        failure {
            echo "Pipeline failed. Please check the logs for errors."
        }

        always {
            echo "Cleaning up and archiving the build"
            // Archive test results, logs, or other artifacts
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            junit '**/target/test-*.xml'  // Example for test reports
        }
    }
}
