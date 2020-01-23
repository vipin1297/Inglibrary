node('master'){
   
   stage('git checkout'){
                  git 'https://github.com/vipin1297/Inglibrary.git'
              }
   stage('java build'){
             sh '/opt/maven/bin/mvn clean verify sonar:sonar -Dsonar.password=admin -Dsonar.login=admin'
         }
   stage("build & SonarQube analysis") {
              withSonarQubeEnv('sonar') {
                 sh '/opt/maven/bin/mvn clean deploy'
              }
          }
      
      stage("Quality Gate"){
         withSonarQubeEnv('sonar'){
         sh '/opt/sonar-scanner-3.3.0.1492-linux/bin/sonar-scanner -Dsonar.host.url=http://3.133.106.68:9000/'
          timeout(time: 60, unit: 'SECONDS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }
      }
   stage('Running java backend application'){
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -Dspring.profiles.active=sit -jar $WORKSPACE/target/*.jar &'
         }
}
