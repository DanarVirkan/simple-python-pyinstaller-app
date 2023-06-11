node {
    checkout scm
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
    stage('Deliver') { 
        docker.image('cdrx/pyinstaller-linux:python2').withRun('-v "source:/src/"', '"pyinstaller -F sources/add2vals.py"') {
            archiveArtifacts "17/sources/dist/add2vals"
        }
    }
}
