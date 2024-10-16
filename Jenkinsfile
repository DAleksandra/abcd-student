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
        stage('OSV Scan') {
            steps {
                sh '''
                    osv-scanner scan --lockfile package-lock.json --format json --output results/sca-osv-scanner.json
                '''
            }
         //   post {
         //       always {
            //        defectDojoPublisher(
             //           artifact: '/var/jenkins_home/workspace/DevSecOps/scan-results.txt',
            //            productName: 'Juice Shop',
              //          scanType: 'OSV',
              //            engagementName: 'aleksandra.dura@hitachienergy.com'
               //     )
            //    }
           // }
        }
    }
}