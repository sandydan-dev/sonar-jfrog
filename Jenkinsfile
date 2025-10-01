pipeline {
   agent any

   // MAVEN ENV START
   environment {
      PATH="/opt/maven/bin:$PATH"
   }
   // MAVEN ENV END


   // STAGES START
    stages {

       // BUILD STAGE START
        stage("build stage"){
           steps {
              echo "----- build start -----"
              sh "mvn clean package -Dmaven.test.skip=true"
              echo "----- build end -----"
           }
        }
       // BUILD STAGE END 

       // TEST STAGE START
        stage("test stage"){
           echo "----- test start -----"
           sh "mvn surefire-report:report"
           echo "----- test end ------"
        }
       // TEST STAGE END 
 
       // SONAR ANALYSIS START
        stage("SonarQube Analysis"){
           // SCANNER ENV START
            environment {
               scannerHome = tool "sandy-sonar-scanner"
            }
           // SCANNER ENV END

           // SONAR SERVER START
	    withSonarQubeEnv("sandy-sonar-server"){
                sh "${scannerHome}/bin/sonar-scanner"
            }
           // SONAR SERVER END

        }
       // SONAR ANALYSIS END

    }
   // STAGES END

}
