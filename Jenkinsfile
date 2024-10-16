pipeline {
    agent any

    stages {
        stage('Install Apache') {
            steps {
                script {
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install -y apache2'
                    sh 'sudo systemctl start apache2'
                    sh 'sudo systemctl enable apache2'
                }
            }
        }
        stage('Check Logs') {
            steps {
                script {
                    def logFilePath = '/var/log/apache2/error.log'
                    echo "Checking logs at: ${logFilePath}"

                    def checkLogFileCommand = "sudo ls -l ${logFilePath}"
                    echo "Executing command: ${checkLogFileCommand}"
                    try {
                        sh(script: checkLogFileCommand)
                    } catch (Exception e) {
                        echo "Failed to check log file: ${e.getMessage()}"
                        return 
                    }

                    def checkLogsCommand = "sudo grep -E '4[0-9]{2}|5[0-9]{2}' ${logFilePath}"
                    echo "Executing command: ${checkLogsCommand}"

                    try {
                        def errors = sh(script: checkLogsCommand, returnStdout: true).trim()
                        echo "Errors found: ${errors}"  
                    } catch (Exception e) {
                        echo "Failed to check logs: ${e.getMessage()}"  
                        def exitCode = sh(script: checkLogsCommand, returnStatus: true)
                        echo "Grep command exited with code: ${exitCode}"
                    }
                }
            }
        }
    }
}
