pipeline {
    agent any

    stages {
        //--Build --//
        stage(' Build Backend') {
            steps {
                // gerar no binario

                bat 'mvn clean package -DskipTests=true'
            }
        }

        //--JUnit --//
        stage('Unit Tests') {
            steps {
                // executa nosos testes
                bat 'mvn test'
            }
        }

        //--Sonar --//
        stage('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER' // nome do sonar declarado nas configurações do jenkins - em SonarQube Scanner
            }

            steps {
                withEnv(["JAVA_HOME=${tool 'JAVA_17'}", "PATH=${tool 'JAVA_17'}/bin:${env.PATH}"]) {
                    bat 'java -version'
                    bat 'javac -version'
                }
                // nome definindo para o SonarQube installations nas configurações de sistema do jenkins
                withSonarQubeEnv('SONAR_LOCAL') {
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=Deploy_Back -Dsonar.host.url=http://localhost:9000 -Dsonar.login=c163ea73dffd8fb0214151b4b59770fe234885d2 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**model/**,**Application.java"
                }
            }
        }
    }
}

