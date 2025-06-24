def ONLY_CONFIGURATION_CHANGED = false

pipeline {
    agent any

    environment {

        CONFIG_PATH = 'config/'
        RESOURCE_PATH = 'resources/' 
    }

    stages {
        stage('Check Only Configurations Changed') {
            steps {
                script {
                    // Run git diff to get changed files
                    def changedFiles = sh(
                        script: "git diff --name-only HEAD~1 HEAD",
                        returnStdout: true
                    ).trim().split("\n").findAll { it }

                    echo "Changed files: ${changedFiles}"

                    // || file.startsWith(env.RESOURCE_PATH)
                    // Check if all changed files are within the specified folder
                    def onlyConfigurationChanged = changedFiles.every { file ->
                        file.startsWith(env.CONFIG_PATH) || file.startsWith(env.RESOURCE_PATH)
                    }

                    ONLY_CONFIGURATION_CHANGED = onlyConfigurationChanged

                }
            }
        }
        stage('Conditional Execution') {
            steps {
                script {
                    if (ONLY_CONFIGURATION_CHANGED) {
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