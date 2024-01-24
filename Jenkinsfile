pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Manual Approval') { 
            steps {
                input message: 'Lanjutkan ke tahap Deploy? (klik "Proceed" (melanjutkan eksekusi pipeline ke tahap Deploy) atau "Abort" (menghentikan eksekusi pipeline))' 
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-windows:python3'
                    args "--entrypoint=''"
                }
            }
            steps {
                echo "Install Requirements, Success."
                sh '''
                    cd app/
                    pip install -r requirements.txt
                '''
            }
        }
    }
}
