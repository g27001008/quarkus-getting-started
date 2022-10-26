pipeline {
    agent {
        dockerfile {
            filename "src/test/jmeter/Dockerfile"
        }
    }
    
    environment {
        WORKSPACE = pwd()
        JMETER_OUT_DIR = "${WORKSPACE}/jmeter-outputs"
    }
    
    stages {
        stage("load-test") {
            
            steps {
                
                sh "mkdir -p ${JMETER_OUT_DIR}"
                sh "jmeter -n -t src/test/jmeter/TestQuarkusGettingStarted.jmx -l ${JMETER_OUT_DIR}/test-result.jtl -e -o ${JMETER_OUT_DIR}/reports"
                sh "cat ${JMETER_OUT_DIR}/test-result.jtl"
                                                
                publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: "${JMETER_OUT_DIR}/reports",
                        reportFiles: "index.html",
                        reportName: "JMeter Report"
                ]
            }
        }
    }
}
