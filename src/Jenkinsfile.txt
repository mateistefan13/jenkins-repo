pipeline {
    agent any
 
    stages {
        stage('Build') {
            steps {
                echo 'Se construiește aplicația...'
                sh 'mvn -B -DskipTests clean package'    // exemplu Maven
                // sh 'npm install && npm run build'     // exemplu Node.js
            }
        }
 
        stage('Test') {
            steps {
                echo 'Rulează testele...'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
 
        stage('Deploy') {
            when { branch 'main' }
            steps {
                echo 'Deploy pe staging/production...'
                // sh './deploy.sh'
            }
        }
    }
 
    post {
        success { echo 'Build reușit!' }
        failure { echo 'Build eșuat :(' }
    }
}