def repository = 'https://github.com/vova-pereb/test.git'

void setBuildStatus(String message, String state) {
    step([
            $class: "GitHubCommitStatusSetter",
            reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/vova-pereb/test"],
            contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "jenkins"],
            errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
            statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
    ])
}

node('lbl-akso-dev-002') {
    def mvnHome = tool 'M3'
    def jdk = tool 'JDK-18'

    stage('Preparation') {
        checkout([
                $class                           : 'GitSCM',
                branches                         : [[name: 'vova-pereb-patch-1']],
                doGenerateSubmoduleConfigurations: false,
                extensions                       : [],
                submoduleCfg                     : [],
                userRemoteConfigs                : [[
                                                            credentialsId: 'vova-github-1',
                                                            url          : repository
                                                    ]]
        ])

        env.JAVA_HOME = "${jdk}/jdk-18.0.1.1"
    }

    stage('Build BE web-application') {
        try {
            echo "${payload}"
            echo "${action}"
            //sh "'${mvnHome}/bin/mvn' -DskipTests=true clean package -f backend/pom.xml"
            setBuildStatus('Success', "SUCCESS")
        } catch (e) {
            currentBuild.result = "FAILURE"
            setBuildStatus('Failure', "FAILURE")
        }
    }

}
