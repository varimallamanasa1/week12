pipeline {
    agent any
   
    stages {

        stage('Run Selenium Tests with pytest') {
    steps {
        echo "Running Selenium Tests using pytest"

        // Install Python dependencies
        bat '"C:\Users\MANASA\AppData\Roaming\Python\Python310\Scripts\pip.exe" install -r requirements.txt'

        // Start Flask app (non-blocking)
        bat '"C:\Users\MANASA\AppData\Roaming\Python\Python310\Scripts\pip.exe" app.py'

        // Wait for Flask server to start
        bat 'ping 127.0.0.1 -n 5 > nul'

        // Run Selenium tests
        bat '"C:\Users\MANASA\AppData\Roaming\Python\Python310\Scripts\pip.exe" -m pytest -v'

    }
}


        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t seleniumdemoapp:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                echo "Logging in to Docker Hub"
                bat 'docker login -u varimallamansa1 -p "Manasa@247" '
            }
        }

        stage('push Docker Image to Docker Hub') {
            steps {
                echo "Pushing Docker Image to Docker Hub"
                bat "docker tag seleniumdemoapp:v1 varimallamansa1/sample1:seleniumtestimage"               
                bat "docker push varimallamansa1/sample1:seleniumtestimage"
            }
        }

        stage('Deploy to Kubernetes') {
        steps {
        echo "Deploying to Kubernetes..."
        withEnv(['KUBECONFIG=C:\\Users\\MANASA\\.kube\\config']) {
            bat 'kubectl apply -f deployment.yaml --validate=false'
            bat 'kubectl apply -f service.yaml'
        }
    }
}
    }

    post {
        always {
            echo '🧹 Cleaning up Flask process...'
            // Kill any running python.exe process (Flask server)
            bat 'taskkill /F /IM python.exe /T || exit 0'
        }

        success {
            echo '✅ Pipeline completed successfully!'
        }

        failure {
            echo '❌ Pipeline failed. Please check the logs.'
        }
    }
}
