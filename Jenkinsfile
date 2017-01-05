#!groovy

node {
   // ------------------------------------
   stage 'Build'

   echo 'Configurando variaveis'
   def mvnHome = tool 'M3'
   env.PATH = "${mvnHome}/bin:${env.PATH}"
   echo "var mvnHome='${mvnHome}'"
   echo "var env.PATH='${env.PATH}'"

   echo 'Checkout código SCM'
   sh 'rm -rf *'
   checkout scm
   
   echo 'Compilando'
   sh 'mvn clean compile'
   // ------------------------------------


   // ------------------------------------
   stage 'Test'
   echo 'Executando testes'
   try{
      sh 'mvn verify'
      step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
   }catch(err) {
      step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
      if (currentBuild.result == 'UNSTABLE')
         currentBuild.result = 'FAILURE'
      throw err
   }
   // ------------------------------------


   // ------------------------------------
   stage 'Inspeção de Código'
   echo 'Inspeção contínua de qualidade de código'
   // ------------------------------------
   

   // ------------------------------------
   stage 'Deploy'
   echo 'Geração do artefato para deploy'
   step([$class: 'ArtifactArchiver', artifacts: '**/target/*.jar, **/target/*.war', fingerprint: true])
   // ------------------------------------
   

   // ------------------------------------
   stage 'Testes Funcionais'
   echo 'Testes funcionais da aplicação'
   // ------------------------------------
   
}