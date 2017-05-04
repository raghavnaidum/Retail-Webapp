node {
    stage("Checkout"){
      git url: 'https://github.com/SudhirG85/Retail-Webapp.git'
    }
    stage("Build"){
      bat "mvn clean install"
      step([$class: 'ArtifactArchiver', artifacts: '**/target/*.war', fingerprint: true])
    }
     stage("Code Analysis"){
      bat "C:\\MyApplications\\Sonar\\sonar-scanner-2.8\\bin\\sonar-runner -Dsonar.host.url=http://localhost:9000  -Dsonar.login=admin -Dsonar.password=admin -Dsonar.projectName=Retail-Webapp -Dsonar.projectVersion=1.0 -Dsonar.projectKey=Retail-Webapp -Dsonar.sources=src"
    }
    stage('Deploy') {
      echo 'Deploying..'
      bat 'echo %WORKSPACE%'
      bat 'set CATALINA_HOME=C:\\MyApplications\\apache-tomcat-8.5.9\nCALL C:\\MyApplications\\apache-tomcat-8.5.9\\bin\\shutdown.bat', propagate: 'false'
      bat 'COPY .\\target\\retailone.war C:\\MyApplications\\apache-tomcat-8.5.9\\webapps\\'
      bat 'set CATALINA_HOME=C:\\MyApplications\\apache-tomcat-8.5.9\nCALL C:\\MyApplications\\apache-tomcat-8.5.9\\bin\\startup.bat'
    }
    stage("Test"){
      bat "mvn integration-test"
      step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
      performanceReport compareBuildPrevious: false, configType: 'ART', errorFailedThreshold: 0, errorUnstableResponseTimeThreshold: '', errorUnstableThreshold: 0, failBuildIfNoResultFile: false, modeOfThreshold: false, modePerformancePerTestCase: true, modeThroughput: false, nthBuildNumber: 0, parsers: [[$class: 'JMeterParser', glob: '**/TEST-*.xml']], relativeFailedThresholdNegative: 0.0, relativeFailedThresholdPositive: 0.0, relativeUnstableThresholdNegative: 0.0, relativeUnstableThresholdPositive: 0.0, absoluteFailedThresholdNegative: 0.0, absoluteFailedThresholdPositive: 0.0
    }
}
