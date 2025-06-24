pipeline {
    agent any

    environment {
        ONLY_CONFIGURATION_CHANGED = false // Initialize the flag
        FOLDER_PATH = 'config/'           // Specify the folder to monitor
        COMMIT_FROM =             // Previous commit (you can adjust this)
        COMMIT_TO =            // Latest commit
    }

    stages {
        stage('Echo Test') {
            steps {
                echo 'Hello, Jenkins! This is a test to check the pipeline execution.'
            }
        }

        stage('Check Configuration Changes') {
            steps {
                script {
                    // Run git diff to get changed files
                    def changedFiles = sh(
                        script: "git diff --name-only HEAD~1 HEAD",
                        returnStdout: true
                    ).trim().split("\n")

                    echo "Changed files: ${changedFiles}"

                    // Check if all changed files are within the specified folder
                    def onlyConfigurationChanged = changedFiles.every { file ->
                        file.startsWith(env.FOLDER_PATH)
                    }

                    // Update the environment variable
                    env.ONLY_CONFIGURATION_CHANGED = onlyConfigurationChanged

                    echo "Only configuration changed: ${env.ONLY_CONFIGURATION_CHANGED}"
                }
            }
        }

        stage('Conditional Execution') {
            steps {
                script {
                    if (env.ONLY_CONFIGURATION_CHANGED.toBoolean()) {
                        echo 'Changes were only in the configuration folder. Proceeding...'
                        // Add additional steps for configuration-only changes here.
                    } else {
                        echo 'Changes were outside the configuration folder. Skipping...'
                        // Add steps for other types of changes here.
                    }
                }
            }
        }
    }
}