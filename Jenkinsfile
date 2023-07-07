// Scripted Jenkinsfile Created by Rachmandev
node {
    checkout scm
    withDockerContainer('python:2-alpine') {
        stage('Build') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    withDockerContainer('qnib/pytest') {
        stage('Test') {
            sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }
    stage('Deploy') {
        try {
            input id: 'Deploy', message: 'Lanjutkan ke tahap Deploy ?', ok: 'Deploy!'
            sh 'docker run --rm -v $(pwd)/sources:/src cdrx/pyinstaller-linux:python2 \'pyinstaller -F /src/add2vals.py\''
            archiveArtifacts artifacts: 'sources/add2vals.py', followSymlinks: false
            sh 'docker run --rm -v $(pwd)/sources:/src cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
            sleep time: 1, unit: 'MINUTES'
        } catch (error) {
            sh "chmod +x -R ${env.WORKSPACE}"
            sh './jenkins/scripts/kill.sh'
        }
        // input message: 'Lanjutkan ke tahap Deploy ?'
        // sh 'docker run --rm -v $(pwd)/sources:/src cdrx/pyinstaller-linux:python2 \'pyinstaller -F /src/add2vals.py\''
        // archiveArtifacts artifacts: 'sources/add2vals.py', followSymlinks: false
        // sh 'docker run --rm -v $(pwd)/sources:/src cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
        // sleep time: 1, unit: 'MINUTES'
        }
    
}