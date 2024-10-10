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
                  sh '''
                    docker run --name juice-shop -d --rm \\
                        -p 3000:3000 \\
                        bkimminich/juice-shop
                    sleep 5
                '''
                sh '''
                    docker run --name zap --rm \
                    --add-host=host.docker.internal:host-gateway \
                    -v /mnt/c/Users/duraa/Desktop/DevSecOps/abcd-student/passiveScan:/zap/wrk:rw \
                    -v /mnt/c/Users/duraa/Desktop/DevSecOps/abcd-student/results:/zap/wrk/reports:rw \
                     -t ghcr.io/zaproxy/zaproxy:stable bash -c \
                    "zap.sh -cmd -addonupdate; zap.sh -cmd -addoninstall communityScripts -addoninstall pscanrulesAlpha -addoninstall pscanrulesBeta -autorun /zap/wrk/passive_scan.yaml" 
                '''
            }
            post {
                always {
                    sh '''
                        cat /mnt/c/Users/duraa/Desktop/DevSecOps/abcd-student/results/zap_xml_report.xml
                    '''
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