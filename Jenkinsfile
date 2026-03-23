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

        stage('Compile') {
            steps {
                script {
                    runStage('Compile') {
                        if (new Random().nextInt(4) == 0) { error('compile error: undefined symbol') }
                        echo 'compilation successful'
                    }
                }
            }
        }

        stage('Unit Tests') {
            steps {
                script {
                    runStage('Unit Tests') {
                        if (new Random().nextInt(3) == 0) { error('unit test failed: assertion error in TestUserAuth') }
                        echo 'all unit tests passed'
                    }
                }
            }
        }

        stage('Code Coverage') {
            steps {
                script {
                    runStage('Code Coverage') {
                        if (new Random().nextInt(5) == 0) { error('coverage below threshold: 61% < 80%') }
                        echo 'coverage: 84%'
                    }
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    runStage('Lint') {
                        if (new Random().nextInt(4) == 0) { error('lint: 3 errors found') }
                        echo 'no lint issues'
                    }
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    runStage('Security Scan') {
                        if (new Random().nextInt(5) == 0) { error('vulnerability found: CVE-2024-1234 in dependency') }
                        echo 'no vulnerabilities found'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    runStage('Build Docker Image') {
                        if (new Random().nextInt(4) == 0) { error('docker build failed: layer cache miss') }
                        echo 'image built: app:latest'
                    }
                }
            }
        }

        stage('Integration Tests') {
            steps {
                script {
                    runStage('Integration Tests') {
                        if (new Random().nextInt(3) == 0) { error('integration test failed: timeout connecting to database') }
                        echo 'all integration tests passed'
                    }
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    runStage('Deploy to Staging') {
                        if (new Random().nextInt(5) == 0) { error('deploy failed: staging cluster unreachable') }
                        echo 'deployed to staging'
                    }
                }
            }
        }

        stage('Smoke Tests') {
            steps {
                script {
                    runStage('Smoke Tests') {
                        if (new Random().nextInt(4) == 0) { error('smoke test failed: /health returned 503') }
                        echo 'smoke tests passed'
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

