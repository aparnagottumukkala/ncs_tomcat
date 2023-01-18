pipeline {
    agent any
    environment {
        ROOT_PATH  =  "/apache-tomcat-9.0.58"
        APP_PATH   =  "$ROOT_PATH/webapps"
        TEMP_DIR   =  "${env.WORKSPACE}/web-thymeleaf-war"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Compile..'
                echo "workspace ${env.WORKSPACE}" 
                echo "BuildNumber :: ${env.BUILD_NUMBER}"
                
                echo "generating war file"
                bat "mvn clean package"
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
                sh "ls -ltr ${TEMP_DIR}/sample_${env.BUILD_NUMBER}.war"
                sh "cp ${TEMP_DIR}/sample_${env.BUILD_NUMBER}.war $APP_PATH/sample.war"
                sh "ls -ltr $APP_PATH" 
            }
        }
       stage('Start Catalina') {
           steps {
               echo "Start catalina"
                sh '''

                  cd $ROOT_PATH
                  pwd
                  nohup bin/catalina.sh start &

                '''
           }
       }
    }
}
