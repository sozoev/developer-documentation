#!groovy
import groovy.json.JsonSlurper

node('master'){
    checkout scm

    def payload_obj     = new JsonSlurper().parseText(payload)
    def push_branch_ref = payload_obj.ref
    print push_branch_ref
    def channel         = 'integrations-testing'
    def app_name        = 'developer_documentation'
    payload_obj         = null

    if( push_branch_ref == 'refs/heads/master' ){
        // def docker_container = docker.build( app_name )
        // docker_container.inside {
            notifySlack("${app_name} build and publish starting!", channel)

            stage 'Publish'
            sh 'bundle install'
            sh 'if [ -d "build" ]; then rm -rf "build"; fi'
            // echo "sudo GIT_SSH_COMMAND='ssh -i \$KEY_FILE' rake publish --trace"
            // sh "sudo GIT_SSH_COMMAND='ssh -i \$KEY_FILE' rake publish --trace"
            sh "GIT_SSH_COMMAND='ssh -i /var/lib/jenkins/.ssh/dev_doc_deployer' rake publish --trace"

            notifySlack("${app_name} publish finished!", channel)
        //}
    }
    else{
        notifySlack("${app_name} non-master change pushed.", channel)
    }
}
