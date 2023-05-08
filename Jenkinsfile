pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '1'))
    }
   parameters {
        text(name: 'DOMAINS', description: 'Enter a list of domains to check DNSSEC for, one domain per line', defaultValue: 'example.com\nanto.online', trim: true)
    }
    stages {
        stage('Install Python3') {
            steps {
                sh 'sudo apt-get update && sudo apt-get install -y python3 python3-pip'
            }
        }
        stage('Clone repository') {
            steps {
                git 'https://github.com/AntoOnline/python-dnssec-checker.git'
            }
        }
        stage('Install requirements') {
            steps {
                sh 'sudo pip3 install -r requirements.txt'
            }
        }
        stage('Check DNSSEC for each domain') {
            steps {
                script {
                    def domains = params.DOMAINS.split('\n').findAll { it.trim() != '' }
                    for (def domain : domains) {
                        def result = sh(script: "python3 dnssec_checker.py --domain ${domain.trim()}", returnStdout: true).trim()
                        if (result != 'OK: there is a valid dnssec self-signed key for the domain') {
                            error "DNSSEC check failed for domain: ${domain}"
                        }
                    }
                }
            }
        }
    }
}
