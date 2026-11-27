pipeline {
    agent { label 'vcm' }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-21-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    options {
        timestamps()
        timeout(time: 20, unit: 'MINUTES')
    }

    stages {
        stage('Build') {
            steps {
                sh 'java -version && javac -version && mvn -version'
                sh 'mvn -B clean install -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-lab') {
                    sh '''
                      mvn -B org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
                        -Dsonar.projectKey=bankapp-gate \
                        -Dsonar.projectName=bankapp-gate \
                        -Dsonar.branch.name=${CHANGE_BRANCH:-$BRANCH_NAME}
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always { echo "Finished: ${currentBuild.currentResult}" }
    }
}
