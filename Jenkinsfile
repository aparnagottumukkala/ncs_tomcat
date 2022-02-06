pipeline {
    agent any
    environment {
        ROOT_PATH  =  "/Users/manojvarma/Desktop/NCS_Demo/apache-tomcat-9.0.58"
        APP_PATH   =  "$ROOT_PATH/webapps"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Compile..'
                sh 'printenv'
                echo "workspace ${env.WORKSPACE}" 
                echo "BuildNumber :: ${env.BUILD_NUMBER}"
                
                echo "generating war file"
                sh "cd ${env.WORKSPACE}/sample"
                sh '''
                   pwd
                   jar -cvf sample_${BUILD_NUMBER}.war *
                   ls -ltr
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo "Stop catalina"
                sh '''

                  cd $ROOT_PATH
                  pwd
                  ./bin/catalina.sh stop

                '''

                echo 'Deploying....'
                echo "Remove older war files"
                sh '''
                   ls -ltr $APP_PATH
                   rm -rf $APP_PATH/sample*

                   echo "After removing older application (sample)"
                   ls -ltr $APP_PATH/
                   
                   echo "deploy new application"
                '''
                sh "ls -ltr ${env.WORKSPACE}/sample_${env.BUILD_NUMBER}.war"
                sh "cp ${env.WORKSPACE}/sample_${env.BUILD_NUMBER}.war $APP_PATH/" 
            }
        }
       stage('Start Catalina') {
           steps {
               echo "Start catalina"
               sh "cd $ROOT_PATH"
               sh "./bin/catalina.sh start"
           }
       }
    }
}
