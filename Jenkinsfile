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
                rm -rf html-report
                rm -f result.jtl
                jmeter -n -t jenkinsIntegrationTest.jmx -l result.jtl -j jmeter.log -e -o html-report
                echo "Listing html-report directory:"
                ls -l html-report
                echo "Checking if index.html exists:"
                ls -l html-report/index.html || echo "index.html not found!"
                echo "First 20 lines of result.jtl:"
                head -20 result.jtl || echo "result.jtl not found!"
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
            // Raporu doğru klasörden arşivle
            archiveArtifacts artifacts: 'html-report/**', allowEmptyArchive: true
        }
    }
}
