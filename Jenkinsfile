pipeline{
  agent any
  tools{
       maven 'mymaven'
       jdk 'jdk11'
   }
  stages {
    stage('Build'){
       steps {
                sh'mvn clean package'
            }
     }
    stage('Test'){
      steps{
        sh 'mvn test'
      }
      }
     stage('collect artifact'){
     steps{
     archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
     }
     }
     stage('deploy to artifactory')
     {
     steps{
     
     rtUpload (
    serverId: 'artifactory-server',
    spec: '''{
          "files": [
            {
              "pattern": "target/*.jar",
              "target": "art-doc-dev-loc"
            }
         ]
    }''',
 
    buildName: 'holyFrog',
    buildNumber: '42'
)
     }}
     stage('download from artifactory')
         {
            steps{
     
                rtDownload (
                    serverId: 'artifactory-server',
                    spec: '''{
                    "files": [
                         {
                             "pattern": "art-doc-dev-loc/",
                             "target": ""
                        }
                     ]
                }''',
 
)
     }}

     stage("deploy to ec2"){
       steps{
            sshagent(['f674a595-aa5a-4e23-90fb-eb8ee9341dfe']){
                    sh 'scp -r target/*.jar ubuntu@54.190.49.106:/home/ubuntu/artifacts'
       }
     }
     } 
      
  
  }
  }