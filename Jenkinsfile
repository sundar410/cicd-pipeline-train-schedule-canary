node {
    stage ('Preperation'){
        git credentialsId: '', url: 'https://github.com/sundar410/cicd-pipeline-train-schedule-canary.git'

    }
    stage ('Build'){
        sh'pwd'
        echo'Running Build Automation'
        sh'./gradlew build --no-daemon'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
    }
    stage('Build Docker Image') {
        app = docker.build("sundar41097/train-schedule")
        app.inside {
           sh 'echo Hello, World!'
        }
    }
    stage('Push Docker Image') {
          docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
         } 
    }
    stage('CanaryDeploy') 
         environment { 
                CANARY_REPLICAS = 1
         }
         kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube-canary.yml',
                    enableConfigSubstitution: true
         )
           
        }
    stage('DeployToProduction') {
        environment { 
                CANARY_REPLICAS = 0
        }
        input 'Deploy to Production?'
        milestone(1)
        kubernetesDeploy(
            kubeconfigId: 'kubeconfig',
            configs: 'train-schedule-kube.yml',
            enableConfigSubstitution: true
        ) 
        kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true
        )
               
       }
}    
    
