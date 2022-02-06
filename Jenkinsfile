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
                echo 'Workspace :: ' ${env.WORKSPACE} 
                echo 'BuildNumber ::' ${env.BUILD_NUMBER}
                sh "ls -ltr"
                echo "generating war file"
                cd $WORKSPACE/sample
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
                echo "Stop catalina"
                sh ".$ROOT_PATH/bin/catalina.sh stop"

                echo 'Deploying....'
                echo "Remove older war files"
                sh '''
                   ls -ltr $APP_PATH
                   rm -rf $APP_PATH/sample*

                   echo "After removing older application (sample)"
                   ls -ltr $APP_PATH/
                   
                   echo "deploy new application"
                   cp $WORKSPACE/sample/sample_${BUILD_NUMBER}.war $APP_PATH
                '''
            }
        }
       stage('Start Catalina') {
           steps {
               echo "Start catalina"
               sh ".$ROOT_PATH/bin/catalina.sh start"
           }
       }
    }
}
