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
                    pwd
                    ls /var/jenkins_home/workspace/DevSecOps/passiveScan
                    docker run --name zap --rm \
                        --add-host=host.docker.internal:host-gateway \
                        -v /var/jenkins_home/workspace/DevSecOps/abcd-student/.zap/passive_scan.yaml:/home/zap/wrk:rw \
                        -t ghcr.io/zaproxy/zaproxy:stable bash -c \
                        "ls /home/zap/wrk" 
                '''
            }
            post {
                always {
                    sh '''
                        docker cp zap:/zap/wrk/zap_html_report.html ${WORKSPACE}/results/zap_html_report.html
                        docker cp zap:/zap/wrk/zap_xml_report.xml ${WORKSPACE}/results/zap_xml_report.xml
                        docker stop zap juice-shop
                    '''
                }
            }
        }
    }
}