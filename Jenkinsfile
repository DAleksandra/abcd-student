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
                    trufflehog git --branch main ./ --json results/trufflehog.json
                '''
            }
           // post {
              //  always {
          //          defectDojoPublisher(
          //              artifact: '/var/jenkins_home/workspace/DevSecOps/results/trufflehog.json',
          //              productName: 'Juice Shop',
            //            scanType: 'Trufflehog Scan', 
          //              engagementName: 'aleksandra.dura@hitachienergy.com'
             //       )
           //     }
           // }
        }
    }
}