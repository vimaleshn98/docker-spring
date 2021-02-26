pipeline{
  agent any
  tools{
       maven 'mymaven'
       jdk 'JDK11'
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
            sshagent(['fb6287dd-0b69-406a-88b3-16309ea4d52a']){
                    sh 'scp -r /var/jenkins_home/workspace/artifactory-pipeline/target/*.jar ubuntu@18.221.138.29:/home/ubuntu/artifact'
        }
       }
     }
       
      
  
  }
  }