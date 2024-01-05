pipeline {
    agent any

    environment {
        JAVA_HOME = "${tool 'JAVA_11'}"
        PATH = "${tool 'JAVA_11'}/bin:${env.PATH}"
        SCANNER_HOME = tool 'SONAR_SCANNER'
    }

    stages {
        stage('Build Backend') {
            steps {
                bat 'java -version'
                bat 'javac -version'
                bat 'mvn clean package -DskipTests=true'
            }
        }

        stage('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Sonar Analysis') {
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                    bat "${SCANNER_HOME}/bin/sonar-scanner -e -Dsonar.projectKey=Deploy_Back -Dsonar.host.url=http://localhost:9000 -Dsonar.login=c163ea73dffd8fb0214151b4b59770fe234885d2 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**model/**,**Application.java"
                }
            }
        }
    }
}
