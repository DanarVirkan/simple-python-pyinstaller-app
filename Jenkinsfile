node {
    checkout scm
    sh 'pwd'
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            try {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            } finally {
                junit 'test-reports/results.xml'
            }
        }
    }
    stage('Manual Approval') {
        docker.image('cdrx/pyinstaller-linux:python2').withRun('-v "$(pwd):/src/"', '"pyinstaller -F sources/add2vals.py"') {
            archiveArtifacts "dist/add2vals"
        }

        input(message: "Lanjutkan ke tahap Deploy?")
    }
    stage('Deploy') {
        sh 'dist/add2vals 2 4'
        sleep(time: 1, unit: 'MINUTES')
    }
}
