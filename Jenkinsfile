#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    print 'KEY IS' 
    print JWT_KEY_CRED_ID
    print HUB_ORG
    print SFDC_HOST
    print CONNECTED_APP_CONSUMER_KEY
    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

withCredentials( [ file( credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file') ] ) {

    // temporary workaround pending resolution to this issue https://github.com/forcedotcom/cli/issues/81
    sh returnStatus: true, script: "cp ${jwt_key_file} ./server.key"

    echo("Authenticate To Dev Hub...")
    script {
        rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY_DH} --username ${SFDX_DEV_HUB_USERNAME} --jwtkeyfile server.key --setalias ${SFDX_DEV_HUB_ALIAS} --setdefaultdevhubusername --instanceurl ${SFDX_DEV_HUB_HOST}"
        if (rc != 0) { error "hub org authorization failed" }
    }
}
}