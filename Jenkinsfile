// Variables that are not declared are added
// to the script binding.
PUBLISH_COMMIT = 'chore: publish [skip ci]'
MAKE_RELEASE_COMMIT = 'chore: make release'
MAX_COMMIT_MSG_LEN = 100

// Generate Changelog
def getChangeString() {
    def changeString = ''
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

println "Changeset size: ${currentBuild.changeSets.size()}"
println "Items[0]: ${currentBuild.changeSets[0].items[0]}"

// Execute this before anything else, including requesting any time on an agent
if (currentBuild.rawBuild.getCauses().toString().contains('BranchIndexingCause')) {
    println 'INFO: Build skipped due to trigger being Branch Indexing'
    return
}