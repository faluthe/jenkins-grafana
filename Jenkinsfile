pipeline {
    agent any
    stages {
        stage('Flaky Test') {
            steps {
                script {
                    def roll = Math.random()
                    if (roll < 0.4) {
                        error("Simulated failure! Roll: ${roll}")
                    }
                    echo "Passed! Roll: ${roll}"
                }
            }
        }
    }
}

