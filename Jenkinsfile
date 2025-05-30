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
            // 1. Capture the path returned by the 'tool' step.
            def gradleHome = tool 'Gradle'

            // 2. Initialize PATH with the Jenkins-installed Gradle's bin directory FIRST,
            //    then append the *system's default PATH*.
            //    It's crucial to make sure the Jenkins-installed path comes first.
            //    We get the *original* system PATH from env.PATH, which still holds it before our modification.
            env.PATH = "${gradleHome}/bin:${env.PATH}"

            // Debug statements (keep these for verification in the next run)
            sh "echo 'Updated PATH: ${env.PATH}'"
            sh "ls -l ${gradleHome}/bin/gradle || echo 'ERROR: Jenkins-installed Gradle executable not found at: ${gradleHome}/bin/gradle'"
            sh "ls -l /bin/gradle || echo 'WARNING: System /bin/gradle not found or linked (which is fine if we use Jenkins-installed)'"
            sh "java -version || echo 'WARNING: Java not found or configured for the environment.'"

            // 3. Run the Gradle build command from the correct directory.
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
