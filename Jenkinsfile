node('maven') {
  stage('Build') {
    sh " oc whoami"
    sh " pwd ; id;"
    git ref: pipeline
    git url: "https://github.com/Heesun-Yang/simple.git"
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
