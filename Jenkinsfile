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
    stage('Deliver') { 
        docker.image('cdrx/pyinstaller-linux:python2').withRun('-v "$(pwd):/src/"', '"pyinstaller -F sources/add2vals.py"') {
            // dir()
            sh 'pwd'
            sh 'ls'
            sh 'ls sources'
            archiveArtifacts "dist/add2vals"
        }
    }
}
