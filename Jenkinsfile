node {
    scm checkout
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }
    stage('Test') {
        docker.image('qnib/pytest') {
            try {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            } finally {
                junit 'test-reports/results.xml'
            }
        }
    }
    stage('Deliver') { 
        dir(path: env.BUILD_ID) { 
            try {
                unstash(name: 'compiled-results') 
                sh "docker run --rm -v \$(pwd)/sources:/src cdrx/pyinstaller-linux:python2 'pyinstaller -F add2vals.py'" 
            } catch(Exception e) {

            }
            archiveArtifacts "${env.BUILD_ID}/sources/dist/add2vals" 
            sh "docker run --rm -v \$(pwd)/sources:/src cdrx/pyinstaller-linux:python2 'rm -rf build dist'"
        }
    }
}
