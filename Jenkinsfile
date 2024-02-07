pipeline {
    agent any
    environment {
        JAVA_HOME = 'Java11' // Set default JAVA_HOME
    }
    stages {
        stage('Checkout Repository') {
            steps {
                checkout scm
            }
        }

        stage('Set up Java') {
            steps {
                script {
                    // Configure Java tool
                    def javaTool = tool name: 'Java11', type: 'jdk'
                    if (javaTool == null) {
                        error "Java11 JDK not found. Please configure it in Jenkins."
                    } else {
                        env.JAVA_HOME = javaTool
                        sh '${JAVA_HOME}/bin/java -version'
                    }
                }
            }
        }

        stage('Create Firman Directory') {
            steps {
                sh 'sudo mkdir -p /usr/firman'
            }
        }

        stage('Move CSV file to Firman directory') {
            steps {
                sh 'sudo mv tenaga_kesejahteraan.csv /usr/firman/'
            }
        }

        stage('Search CSV File') {
            steps {
                script {
                    def csv_file = sh(script: 'find /usr/firman -type f -name "tenaga_kesejahteraan.csv"', returnStdout: true).trim()
                    if (csv_file) {
                        echo "File tenaga_kesejahteraan.csv found: $csv_file"
                    } else {
                        error "File tenaga_kesejahteraan.csv not found."
                    }
                }
            }
        }

        stage('Set execute permission for TalendTesting_run.sh') {
            steps {
                dir(path: 'TalendLinux/TalendTesting') {
                    sh 'chmod +x TalendTesting_run.sh'
                }
            }
        }

        stage('Run Talend Job') {
            steps {
                sh './TalendLinux/TalendTesting/TalendTesting_run.sh'
            }
        }
    }
}
