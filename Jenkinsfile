pipeline {
    agent any  // Use any available agent

    tools {
        gradle 'Gradle'  // Ensure this matches the name configured in Jenkins
        jdk 'JDK'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/karthik-08-rgb/GradleApp.git'
            }
        }

        stage('Build') {
    steps {
        script {
            // Get the path to the Gradle installation provided by Jenkins' tool auto-installation
            def gradleHome = tool 'Gradle' // 'Gradle' is the name configured in Jenkins Global Tool Configuration
            env.PATH = "${gradleHome}/bin:${env.PATH}"
            // Now, ensure you're in the correct workspace directory
            dir("${workspace}") { // Ensure you are in the repository's root
                sh 'gradle build'
            }
        }
    }
}

       stage('Test') {
           steps {
               sh 'gradle test'  // Run unit tests
           }
        }

              
        stage('Run Application') {
            steps {
                // Start the JAR application
                sh 'gradle run'
            }
        }

        
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
