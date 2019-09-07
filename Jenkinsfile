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

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Create Package') {

            rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            if (rc != 0) { error 'hub org authorization failed' }
            
            chatterPost: "This is a Chatter post from a pipeline! ${env.JOB_NAME} ${env.BUILD_DISPLAY_NAME}", credentialsId: 'e6139670-10b1-485d-a418-6e34ba7e5901', recordId: 'SOME_RECORD_ID'
            
            // need to pull out assigned username
             // rc = bat returnStdout: true, script: "sfdx force:source:deploy --manifest manifest/package.xml -u ${HUB_ORG}"
            printf rc    
        }
        
        //stage('Push To Test Org') {
            //rc = bat returnStatus: true, script: "sfdx force:source:push --targetusername ${SFDC_USERNAME}"
            //if (rc != 0) {
                //error 'push failed'
            //}
            // assign permset
            //rc = bat returnStatus: true, script: "sfdx force:user:permset:assign --targetusername ${SFDC_USERNAME}"
            //if (rc != 0) {
                //error 'permset:assign failed'
            //}
        //}

        //stage('collect results') {
          //  junit keepLongStdio: true, testResults: 'tests/**/*-junit.xml'
        //}
    }
}
