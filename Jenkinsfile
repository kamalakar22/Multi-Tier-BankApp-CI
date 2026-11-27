pipeline {
    agent { label 'vcm' }

    stages {
        stage('Hello') {
            steps ('Build and test'){
                script {
                    sh 'mvn -B clean verify'
                }
            }
        }
    }
}
