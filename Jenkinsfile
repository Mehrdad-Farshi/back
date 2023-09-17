pipeline {
    agent any
    triggers {
    pollSCM('* * * * *')
    }
    environment{
        repo_name = 'back'
        no_ci = false

    }
    stages {
        stage ('outside'){
            steps{
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') { 
                    script{
                        def merge_message = sh(returnStdout: true, script: 'git log -n 1 --pretty=%B').trim()
                        env.mergeCommitMessage = merge_message                         
                        def no_ciRegex = /.*\bno_ci\b.*/
                        if(mergeCommitMessage =~ no_ciRegex){
                            def no_citmp = true
                            env.no_ci = no_citmp
                        }
                    }

                }
            }

        }
        stage('no_ci') {
            when{
                expression{
                    return env.no_ci == 'false'
                }
            }
            steps {
                echo "no_ci : ${no_ci}"
            }
        }
    }
}