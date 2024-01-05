pipeline {
    agent any

    environment {
        JAVA_HOME = "${tool 'JAVA_11'}"
        PATH = "${tool 'JAVA_11'}/bin:${env.PATH}"
        SCANNER_HOME = tool 'SONAR_SCANNER'
        SONARQUBE_LOCAL = 'SONAR_LOCAL'
        SONARQUBE_QG = 'SONAR_LOCAL_QG'  // Nome da configuração do Quality Gate no Jenkins
        SONARQUBE_URL = 'http://localhost:9000/'  // URL do seu servidor SonarQube
        SONARQUBE_TOKEN = 'c163ea73dffd8fb0214151b4b59770fe234885d2'  // Token de acesso do SonarQube
        TOMCAT_LOGIN = 'TOMCAT_LOGIN'
        TOMCAT_URL = 'http://localhost:8001/'
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
                    withSonarQubeEnv(SONARQUBE_LOCAL) {
                        bat "${SCANNER_HOME}/bin/sonar-scanner -e -Dsonar.projectKey=Deploy_Back -Dsonar.host.url=${SONARQUBE_URL} -Dsonar.login=${SONARQUBE_TOKEN} -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**model/**,**Application.java"
                    }
            }
        }

        stage('Quality Gate') {
            steps {
                sleep(5)
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Deploy Back-End') {
            steps {
                deploy adapters: [tomcat8(credentialsId: "${TOMCAT_LOGIN}", path: '', url: "${TOMCAT_URL}")], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }

       
    }
}

