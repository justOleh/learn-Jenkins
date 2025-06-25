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

                    // Check if all changed files are within the specified folder
                    def onlyConfigurationChanged = changedFiles.every { file ->
                        file.startsWith(env.CONFIG_PATH) || file.startsWith(env.RESOURCE_PATH)
                    }

                    ONLY_CONFIGURATION_CHANGED = onlyConfigurationChanged
                }
            }
        }

        stage('Conditional Execution pipeline test') {
            when {
                expression {
                    !ONLY_CONFIGURATION_CHANGED
                }
            }
            steps {
                script {
                    echo 'Changes were outside the configuration folder. Running test stage...'
                }
            }
        }

        stage('Conditional Execution pipeline deployment') {
            when {
                expression {
                    !ONLY_CONFIGURATION_CHANGED
                }
            }
            steps {
                script {
                    echo 'Changes were outside the configuration folder. Running deployment stage...'
                }
            }
        }

        stage('Conditional Execution pipeline finalizing') {
            steps {
                script {
                    if (ONLY_CONFIGURATION_CHANGED) {
                        echo 'ONLY_CONFIGURATION_CHANGED is true. Skipping earlier stages and proceeding with finalizing...'
                    } else {
                        echo 'Changes were outside the configuration folder. Proceeding with finalizing stage...'
                    }
                }
            }
        }
    }
}