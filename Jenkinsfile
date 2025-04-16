pipeline {
    agent any  // Runs on any available Jenkins agent
    
    tools {
        maven 'Maven-3.9.6'  // Name of Maven installation in Jenkins
        jdk 'JDK-17'        // Name of JDK installation in Jenkins
    }
    
    stages {
        // Stage 1: Fetch code from Git
        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/yourusername/your-repo.git'
            }
        }
        
        // Stage 2: Build the JAR with Maven
        stage('Build') {
            steps {
                sh 'mvn clean package'  // Compiles, tests, and packages the JAR
            }
        }
        
        // Stage 3: Run tests (optional)
        stage('Test') {
            steps {
                sh 'mvn test'  // Runs unit tests
            }
        }
        
        // Stage 4: Deploy using systemd service
        stage('Deploy') {
            steps {
                script {
                    // Copy the new JAR to the deployment directory
                    sh 'sudo cp target/your-app.jar /opt/yourapp/'
                    
                    // Restart the systemd service
                    sh 'sudo systemctl restart yourapp'
                    
                    // Verify service status (optional)
                    sh 'sudo systemctl status yourapp --no-pager'
                }
            }
        }
    }
    
    post {
        // Always clean the workspace after build
        always {
            cleanWs()
        }
        
        // Send email on failure (requires Email Extension plugin)
        failure {
            emailext body: 'Build failed! Check ${BUILD_URL}',
                    subject: 'Jenkins Build Failed: ${JOB_NAME}',
                    to: 'dev-team@example.com'
        }
    }
}
