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
    }
}
