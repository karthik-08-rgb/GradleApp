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
            // 1. Capture the path returned by the 'tool' step into a Groovy variable.
            //    The 'tool' step (when used this way) makes the path available.
            def gradleHome = tool 'Gradle'

            // 2. Prepend the Gradle 'bin' directory to the PATH environment variable
            //    for the current execution environment.
            //    This ensures 'gradle' command will find the correct executable.
            env.PATH = "${gradleHome}/bin:${env.PATH}"

            // 3. (Optional but recommended) Add debug statements to verify PATH and existence
            sh "echo 'Current PATH: ${env.PATH}'"
            sh "ls -l ${gradleHome}/bin/gradle || echo 'ERROR: Jenkins-installed Gradle executable not found at: ${gradleHome}/bin/gradle'"
            sh "ls -l /bin/gradle || echo 'WARNING: System /bin/gradle not found or linked (which is fine if we use Jenkins-installed)'"
            sh "java -version || echo 'WARNING: Java not found or configured for the environment.'"


            // 4. Run the Gradle build command from the correct directory.
            dir("${workspace}") {
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
