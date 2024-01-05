pipeline {
    agent any
    stages {
        //--Build --//
        stage(' Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
            //--JUnit --//
        stage('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }
    }
}
