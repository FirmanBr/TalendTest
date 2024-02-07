pipeline {
    agent {
        label 'ubuntu-latest'
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
                    // Setup Java 11
                    env.JAVA_HOME = tool 'Java11'
                    sh 'java -version'
                }
            }
        }
        
        stage('Create Firman Directory') {
            steps {
                sh 'sudo mkdir /usr/firman'
            }
        }
        
        stage('Move CSV file to TalendTesting directory') {
            steps {
                sh 'sudo mv tenaga_kesejahteraan.csv /usr/firman/'
            }
        }
        
        stage('Search CSV File') {
            steps {
                script {
                    def csv_file = sh(script: 'find . -type f -name "tenaga_kesejahteraan.csv"', returnStdout: true).trim()
                    if (csv_file) {
                        echo "File tenaga_kesejahteraan.csv ditemukan: $csv_file"
                    } else {
                        error "File tenaga_kesejahteraan.csv tidak ditemukan."
                    }
                }
            }
        }
        
        stage('Set execute permission for TalendTesting_run.sh') {
            steps {
                dir('TalendLinux/TalendTesting') {
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
