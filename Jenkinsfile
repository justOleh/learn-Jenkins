pipeline {
    agent any

    environment {
        ONLY_CONFIGURATION_CHANGED = false
        CONFIG_PATH = 'config/'
        RESOURCE_PATH = 'resources/' 
    }

    stages {
        stage('Check Configuration Changes') {
            steps {
                script {
                    // Run git diff to get changed files
                    def changedFiles = sh(
                        script: "git diff --name-only HEAD~1 HEAD",
                        returnStdout: true
                    ).trim().split("\n").findAll { it }

                    echo "Changed files: ${changedFiles}"

                    // // Check if all changed files are within the specified folder
                    // def onlyConfigurationChanged = changedFiles.every { file ->
                    //     file.startsWith(env.FOLDER_PATH)
                    // }

                    // // Update the environment variable
                    // env.ONLY_CONFIGURATION_CHANGED = onlyConfigurationChanged

                    // echo "Only configuration changed: ${env.ONLY_CONFIGURATION_CHANGED}"
                }
            }
        }
    }

    //     stage('Conditional Execution') {
    //         steps {
    //             script {
    //                 if (env.ONLY_CONFIGURATION_CHANGED.toBoolean()) {
    //                     echo 'Changes were only in the configuration folder. Proceeding...'
    //                     // Add additional steps for configuration-only changes here.
    //                 } else {
    //                     echo 'Changes were outside the configuration folder. Skipping...'
    //                     // Add steps for other types of changes here.
    //                 }
    //             }
    //         }
    //     }
    // }
}