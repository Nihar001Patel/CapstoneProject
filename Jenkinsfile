pipeline {
    agent any

     environment{
       registryCredential = 'ecr:ap-south-1:202051139_Capstone'
       appRegistry = "659882375431.dkr.ecr.ap-south-1.amazonaws.com/202051139_capstoneproject"
       capstoneRegistry = "https://659882375431.dkr.ecr.ap-south-1.amazonaws.com"
       cluster = "202051139_CapstoneProject"
        service = "202051139_service101"
   }

    stages {
        stage('Clone Website') {
            steps {
                git url:'https://github.com/Nihar001Patel/CapstoneProject'
            }
        }

       stage("Docker Build Image"){
           steps{
             script{
                dockerImage = docker.build(appRegistry+ ":$BUILD_NUMBER",".")
             }
           }
      }

       stage('Test Website') {
            steps {
                // Run tests on the website
                sh 'echo "Running tests on the website"'
            }
       }

       stage("Upload App Image"){
         steps{
            script{
                docker.withRegistry(capstoneRegistry, registryCredential){
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                }
            }
         }
      }

      stage('Deploy to ecs'){
         when {
                branch "master"
         }
         steps{
            withAWS(credentials: '202051139_Capstone', region: 'ap-south-1'){
                sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
            }
         }
      }

    }
}
