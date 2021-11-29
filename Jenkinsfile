node {
    stage ('Preperation'){
        git credentialsId: '', url: 'https://github.com/sundar410/cicd-pipeline-train-schedule-canary.git'

    }
    stage ('Build'){
        sh'pwd'
        echo'Running Build Automation
        sh'./gradlew build --no-daemon'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
    }
}    
    
