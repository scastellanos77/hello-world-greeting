node('docker') {
  stage('Poll') {
  checkout scm
}
stage('Build & Unit test'){
  mvn clean verify -DskipITs=true;
  junit '**/target/surefire-reports/TEST-*.xml'
  archive 'target/*.jar'
}
stage('Static Code Analysis'){
  sh 'pwd ';
}
stage ('Integration Test'){
  sh 'mvn clean verify -Dsurefire.skip=true';
  junit '**/target/failsafe-reports/TEST-*.xml'
  archive 'target/*.jar'
}
stage ('Publish'){
  def server = Artifactory.server 'Default Artifactory Server'
  def uploadSpec = """{
  "files": [
  {
    "pattern": "target/hello-0.0.1.war",
    "target": "example-project/${BUILD_NUMBER}/",
    "props": "Integration-Tested=Yes;Performance-Tested=No"
  }
  ]
  }"""
  server.upload(uploadSpec)
  }
}
