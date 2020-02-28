#!groovy?
node(){
    compile()
}

def compile(){
 stage('CleanUp') {
  cleanWs()
 }
    stage('Checkout') {
            checkout scm 
    }
    stage('Compile') {
            def pom_version = version()
            mvn "compile"  
    }
    stage('Test') {
            def pom_version = version()
            mvn "test"  
    }
    stage('Package') {
            def pom_version = version()
            mvn "package"  
    }
    stage('Nexus Upload'){
           sh 'curl -v -u admin:admin --upload-file gameoflife-web/target/gameoflife.war http://127.0.0.1:8081/repository/artifacts/demoproject/'
    }
    stage('Sonar Analysis'){
           mvn 'sonar:sonar  -Dsonar.login=admin -Dsonar.password=admin -Dsonar.url=http://127.0.0.1:9000/sonar'
    }
}

def mvn(String goals) {
    def mvnHome = tool "maven"
    withEnv(["PATH+MAVEN=${mvnHome}/bin"]) {
        sh "mvn ${goals}"
    }
}

def version() {
    pom = readMavenPom file: 'pom.xml'
    return pom.version
}
