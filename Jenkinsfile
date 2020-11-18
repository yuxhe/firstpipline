node('master') {
 withMaven(maven:'M3') {
  stage('Checkout') {
   git url: 'https://github.com/piomin/sample-spring-cloud-comm.git', credentialsId: '19911663-4d7c-47cb-93b7-2ff07e50178b',   branch: 'feign_with_discovery'
  }

  stage('Build') {
   dir('account-service') {
    sh 'mvn clean install'
   }
   //def pom = readMavenPom file:'pom.xml'
   //print pom.version  pom.version
   env.version = 3.0
   currentBuild.description = "Release: ${env.version}"
  }

  stage('Image') {
   dir ('account-service') {
       //sh '''
       //    docker -H tcp://192.168.220.153:2375  build -t piomin/account-service:3.0  . 
       //   '''
      def app = docker.build "piomin/account-service:${env.version}"
	  //docker build -t piomin/account-service:3.0
      //app.push()
   }
  }

  stage ('Run') {
    docker.image("piomin/account-service:${env.version}").run('-p 8091:8091 -m 256M -e EUREKA_DEFAULT_ZONE=http://discovery:8761/eureka -d --name account --network sample-spring-cloud-network')
   
  }

 }
}