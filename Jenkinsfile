pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Compile..'
                sh 'printenv'
                sh "ls -ltr"
                echo "generating war file"
                sh "jar -cvf sample_${BUILD_NUMBER}.war *"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
