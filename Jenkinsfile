node('maven') {
   stage('checkout tfrs-sample-sonar')
   git url: 'https://github.com/bcgov/tfrs-sonar-scanner.git'

   stage('change to working dir')
   dir('tfrs-sample-project'){

       stage('execute sonar') {

       SONARQUBE_PWD = sh (
         script: 'oc env dc/sonarqube --list | awk  -F  "=" \'/SONARQUBE_ADMINPW/{print $2}\'',
         returnStdout: true
          ).trim()
       echo "SONARQUBE_PWD: ${SONARQUBE_PWD}"

       SONARQUBE_URL = sh (
           script: 'oc get routes -o wide --no-headers | grep sonarqube | awk \'{ print (NF==5) ?  "http://"$2 : "https://"$2 }\'',
           returnStdout: true
              ).trim()
       echo "SONARQUBE_URL: ${SONARQUBE_URL}"

       sh "./gradlew sonarqube -Dsonar.host.url=\"${SONARQUBE_URL}\" -Dsonar.verbose=true --stacktrace"

       }
   }
}
