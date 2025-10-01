// Define the URL of the Artifactory registry
def registry = 'https://trial28jeay.jfrog.io/'

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
          steps { 
             echo "----- test start -----"
             sh "mvn surefire-report:report"
             echo "----- test end ------"
          }
        }
       // TEST STAGE END 
 
       // SONAR ANALYSIS START
        stage("SonarQube Analysis"){
           // SCANNER ENV START
            environment {
               scannerHome = tool "sandy-sonar-scanner"
            }
           // SCANNER ENV END

            steps {
           // SONAR SERVER START
	       withSonarQubeEnv("sandy-sonar-server"){
                  sh "${scannerHome}/bin/sonar-scanner"
               }
           // SONAR SERVER END
            }
        }
       // SONAR ANALYSIS END

       // JFROG START
      
        stage("Jar Publish"){

           steps {

               // SCRIPT START

                  script {

                     echo "----- Jar publish start -----" 

                     def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "artifact-product"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"
                     def version = sh(script: "mvn help:evaluate -Dexpression=project.version -q -DforceStdout", returnStdout: true).trim()
                   
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "target/*.war",
                              "target": "sandysonar-libs-release-local/war-files-verions/${version}/",
                              "flat": "true",
                              "props": "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                    def buildInfo = server.upload(uploadSpec)
                    buildInfo.env.collect()
                    server.publishBuildInfo(buildInfo)
                     
                     echo "----- Jar publish end ------"
                  }

               // SCRIPT END    
    
           }

        }   
     
       // JFROG END


    }
   // STAGES END

}
