pipeline {
    agent any
    
    tools {
        maven 'Maven' // Name of Maven installation in Jenkins
        jdk 'JDK17'    // Name of JDK installation in Jenkins
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/yourusername/your-repo.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Deploy') {
            steps {
                sh '''
                # Stop existing application if running
                pkill -f "java -jar your-app.jar" || true
                
                # Copy new build to deployment directory
                cp target/your-app.jar /opt/yourapp/
                
                # Start application
                cd /opt/yourapp/
                nohup java -jar your-app.jar > app.log 2>&1 &
                '''
            }
        }
    }
    
    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}
