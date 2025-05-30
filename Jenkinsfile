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
            // Assign the path returned by 'tool' to a variable
            def gradleHome = tool 'Gradle' // 'Gradle' should match the name in Jenkins Global Tool Configuration

            // Explicitly add the Gradle bin directory to the PATH environment variable
            env.PATH = "${gradleHome}/bin:${env.PATH}"

            // Ensure you are in the correct workspace directory before running the build command
            dir("${workspace}") {
                // Now, try running gradle build. It should find the correct executable via the updated PATH.
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
