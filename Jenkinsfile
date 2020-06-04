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
    
    boolean isBranchIndexing = false;
    
    //getBuildCauses does not need additional permissions
    //in comparison to currentBuild.rawBuild that needs
    //to be whitelisted
    def buildCauses = currentBuild.getBuildCauses();
        
    buildCauses.each { cause -> 
        
        //cause is a json object and cause._class is a String
        if (cause._class.equals("jenkins.branch.BranchIndexingCause")) {
            isBranchIndexing = true;
        }
    }
    
    return isBranchIndexing;
}

def changeSet = getChangeString()

println "Changeset is: ${changeSet}"

// Execute this before anything else, including requesting any time on an agent
if (isBranchIndexingCause()) {
    println 'INFO: Build skipped due to trigger being Branch Indexing'
    return
}
