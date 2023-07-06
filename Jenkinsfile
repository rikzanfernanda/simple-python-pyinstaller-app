node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'ls -l'
            sh 'python -m py_compile ./sources/add2vals.py ./sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml ./sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }
    stage('Deliver') {
        sh 'docker run --rm -v /var/jenkins_home/workspace/submission-cicd-pipeline-rikzan_fernanda/sources:/src cdrx/pyinstaller-linux:python2 \'pyinstaller -F add2vals.py\''
        archiveArtifacts artifacts: './sources/add2vals.py', followSymlinks: false
        sh 'docker run --rm -v /var/jenkins_home/workspace/submission-cicd-pipeline-rikzan_fernanda/sources:/src cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
    }
}