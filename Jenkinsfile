node('maven') {
  stage('Build') {
    sh " oc whoami"
    sh " pwd ; id;"
    git url: "http://gitlab.octest.sec.samsung.com/octest/test-eap-pipeline.git"
    sh "cp configuration/settings.xml ~/.m2/"
    sh "mvn package"
    sh "ls  *"
  }
  stage('Test') {
    parallel(
      "Cart Tests": {
        sh "curl -s -X POST http://eap-app:8080/"
      },
      "Discount Tests": {
        sh "curl -s -X POST http://eap-app:8080/"
      }
    )
  }
  stage('Build Image') {
    sh "oc start-build eap-app --from-file=target/ROOT.jar --follow"
  }
  stage('System Test') {
    sh " sleep 30"
    sh "curl -s http://eap-app:8080/index.jsp"
  }
}
