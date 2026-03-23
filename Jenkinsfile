def runStage(String stageName, Closure body) {
    try {
        body()
        sh "echo 'stage_result{job_name=\"${env.JOB_NAME}\",stage=\"${stageName}\"} 1' | curl --data-binary @- http://pushgateway:9091/metrics/job/${env.JOB_NAME}"
    } catch(e) {
        sh "echo 'stage_result{job_name=\"${env.JOB_NAME}\",stage=\"${stageName}\"} 0' | curl --data-binary @- http://pushgateway:9091/metrics/job/${env.JOB_NAME}"
        throw e
    }
}

pipeline {
    agent any
    stages {

        stage('Always Passes') {
            steps {
                script {
                    runStage('Always Passes') {
                        echo 'doing work'
                    }
                }
            }
        }

        stage('Always Fails') {
            steps {
                script {
                    runStage('Always Fails') {
                        error('intentional failure')
                    }
                }
            }
        }

    }
}

