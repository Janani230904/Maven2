pipeline {
    agent any

    tools {
        // These MUST match the names in 'Global Tool Configuration'
        maven 'Maven'
        jdk 'JDK21'
    }

    options {
        // Professional practice: add timestamps and a timeout
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
    }

    stages {
        stage('Checkout') {
            steps {
                // Using HTTPS to bypass the 'publickey' error you encountered
                git branch: 'main',
                    url: 'https://github.com/Janani230904/Maven2.git',
                    credentialsId: 'github-token' 
            }
        }

        stage('Build') {
            steps {
                // 'clean' ensures no old artifacts interfere with the new build
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                // Creates the .jar or .war file
                sh 'mvn package'
            }
        }

        stage('Run Application') {
            steps {
                // Ensure com.example.app.App exists in your src directory
                sh 'mvn exec:java -Dexec.mainClass="com.example.app.App"'
            }
        }
    }

    post {
        always {
            // Clean up workspace to save disk space on DBMS-18
            cleanWs()
        }
        success {
            emailext (
                subject: "SUCCESS: ${JOB_NAME} - Build #${BUILD_NUMBER}",
                body: "Build was successful! View details here: ${BUILD_URL}",
                to: "janurgowda239@gmail.com"
            )
        }
        failure {
            emailext (
                subject: "FAILED: ${JOB_NAME} - Build #${BUILD_NUMBER}",
                body: "Build failed. Please check the console output: ${BUILD_URL}",
                to: "janurgowda239@gmail.com"
            )
        }
    }
}
