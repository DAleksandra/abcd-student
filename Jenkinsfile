pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: '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', url: 'https://github.com/DAleksandra/abcd-student', branch: 'main'
                }
            }
        }
        stage('[ZAP] Baseline passive-scan') {
            steps {
                sh '''
                        docker stop zap juice-shop || true
                    '''
            }
            post {
                always {
                    defectDojoPublisher(
                        artifact: '/mnt/c/Users/duraa/Desktop/DevSecOps/abcd-student/results/zap_xml_report.xml',
                        productName: 'Juice Shop',
                        scanType: 'ZAP Scan',
                        engagementName: 'aleksandra.dura@hitachienergy.com'
                    )
                }
            }
        }
    }
}