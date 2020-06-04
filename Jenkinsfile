echo "Será desta?"

MAX_COMMIT_MSG_LEN = 1000
PUBLISH_COMMIT = "DS"

// Generate Changelog
String getChangeString() {
    String changeString = ''
    def changeLogSets = currentBuild.changeSets

    for (int i = 0; i < changeLogSets.size(); i++) {
        def commits = changeLogSets[i].items

        for (int j = 0; j < commits.length; j++) {
            def currentCommit = commits[j]
            truncated_msg = currentCommit.msg.take(MAX_COMMIT_MSG_LEN)

            if ((truncated_msg).startsWith(PUBLISH_COMMIT)) {
                changeString = "${truncated_msg}"
            } else {
                changeString += "- ${truncated_msg} [${currentCommit.author}]\n"
            }
        }
    }

    return changeString
}

boolean isBranchIndexingCause() {
    
    //getBuildCauses does not need additional permissions
    //in comparison to currentBuild.rawBuild that needs
    //to be whitelisted
    def buildCauses = currentBuild.getBuildCauses();
        
    if(buildCauses.size() == 1) {

        def cause = buildCauses[0];

        //cause is a json object and cause._class is a String
        return cause._class.equals("jenkins.branch.BranchIndexingCause");

    }

    return false;
}

// Execute this before anything else, including requesting any time on an agent
//if (isBranchIndexingCause()) {
//    println 'INFO: Build skipped due to trigger being Branch Indexing'
//    return
//}

node {
    ws {
        dir('main') {
            git("https://github.com/boliveira/test.git")
        }
        dir('other') {
            git("https://github.com/boliveira/TestReactNativeApp.git")
        }
        archive '**'
    }
    
    println "currentBuild.changeSets: ${currentBuild.changeSets}"
}

