pipeline {
    agent any
    environment {
        ROOT_PATH  =  "/Users/manojvarma/Desktop/NCS_Demo/apache-tomcat-9.0.58"
        APP_PATH   =  "$ROOT_PATH/webapps"
        TEMP_DIR   =  "${env.WORKSPACE}/web-thymeleaf-war"
        VERSION    =  "1.0.${env.BUILD_NUMBER}" 
    }
    stages {
        stage('Build') {
            steps {
                echo 'Compile..'
                sh 'printenv'
                echo "workspace ${env.WORKSPACE}" 
                echo "BuildNumber :: ${env.BUILD_NUMBER}"
                echo "VERSION :: $VERSION"
                echo "generating war file"
                sh "cd $TEMP_DIR"
                sh '''
                   cd $TEMP_DIR
                   pwd
                   /opt/homebrew/Cellar/maven/3.8.4/bin/mvn clean package
                   ls -ltr target/
                   echo "MD% checksum after generating WAR file"
                   md5 target/mkyong.war
                ''' 
                // sh " /opt/homebrew/Cellar/maven/3.8.4/bin/mvn -B org.codehaus.mojo:versions-maven-plugin:2.5:set -DprocessAllModules -DnewVersion=$VERSION "
                // sh "   /opt/homebrew/Cellar/maven/3.8.4/bin/mvn clean package "
                // sh "   ls -ltr target/ "
            }
        }
        stage('SonaqQube Test') {
            steps {
                echo 'Sonar Testing..'
                sh '''
                cd $TEMP_DIR
                /opt/homebrew/Cellar/maven/3.8.4/bin/mvn verify sonar:sonar -Dsonar.login=bf2e3735918f3ed8e1fd38f954b8a42d32bc656e
                '''
                // sh "/opt/homebrew/Cellar/maven/3.8.4/bin/mvn verify sonar:sonar -Dsonar.login=bf2e3735918f3ed8e1fd38f954b8a42d32bc656e "
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
                   rm -rf $APP_PATH/mkyon*

                   echo "After removing older application (sample)"
                   ls -ltr $APP_PATH/
                   
                   echo "deploy new application"
                '''
                sh "ls -ltr ${TEMP_DIR}/target/mkyong.war"
                sh "cp ${TEMP_DIR}/target/mkyong.war $APP_PATH/"
                sh "ls -ltr $APP_PATH" 
                echo "-----After copying war : Checking MD5------"
                sh "md5 $APP_PATH/mkyong.war " 
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
