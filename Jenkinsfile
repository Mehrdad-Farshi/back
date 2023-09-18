pipeline {
    agent any
    triggers {
    pollSCM('* * * * *')
    }
    // environment{

    // }
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
                not {
                    environment(name: "no_ci", value: "true") 
                }
            }
            steps {
                echo "hello no_ci "
                post {
                        always {
                                emailext body: 'salam app <> build shod ',
                                subject: 'build number : app name :   ',
                                to: 'meh1376@gmail.com'
                        }
                   }
            }
        
        }
    }
}
