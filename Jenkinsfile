pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:/opt/homebrew/bin:/Users/huseyinakcan/Downloads/apache-jmeter-5.6.3/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test Shell') {
            steps {
                sh 'echo $PATH'
                sh 'which sh'
                sh 'which jmeter'
            }
        }

        stage('Run JMeter Test') {
            steps {
                sh '''
                    mkdir -p results
                    rm -rf results/html-report
                    rm -rf results/results.jtl
                    jmeter -n -t jenkinsExecutionFiles/jenkinsIntegrationTest.jmx -l results/results.jtl -e -o results/html-report
                    echo "Listing results/html-report directory:"
                    ls -l results/html-report
                    echo "Checking if index.html exists:"
                    ls -l results/html-report/index.html
                    echo "First 20 lines of results/results.jtl:"
                    head -20 results/results.jtl
                '''
            }
        }

        stage('Publish HTML Report') {
    steps {
        publishHTML(target: [
            allowMissing         : false,
            reportName           : 'JMeterTestReport',
            reportDir            : 'html-report',
            reportFiles          : 'index.html',
            keepAll              : true,
            alwaysLinkToLastBuild: true
        ])
    }
}

    }

    post {
        always {
            // Raporu Jenkins'e arşivle
            archiveArtifacts artifacts: 'results/html-report/**', allowEmptyArchive: true
        }
    }
}
