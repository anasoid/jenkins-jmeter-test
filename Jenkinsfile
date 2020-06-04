// This shows a simple example of how to archive the build output artifacts.

pipeline {
 
    parameters {
        gitParameter branch: '', branchFilter: '.*', defaultValue: 'origin/master', description: 'Which Branch ?', name: 'BRANCH_NAME_PARAM', quickFilterEnabled: true, selectedValue: 'NONE', sortMode: 'NONE', tagFilter: '*', type: 'PT_BRANCH_TAG', useRepository: 'https://github.com/anasoid/jenkins-jmeter-test.git'
    }
	
node {
    stage "Create build output"
    
    // Make the output directory.
    sh "mkdir -p output"

    // Write an useful file, which is needed to be archived.
    writeFile file: "output/usefulfile.txt", text: "This file is useful, need to archive it."

    // Write an useless file, which is not needed to be archived.
    writeFile file: "output/uselessfile.md", text: "This file is useless, no need to archive it."

    stage "Archive build output"
    
    // Archive the build output artifacts.
    archiveArtifacts artifacts: 'output/*.txt', excludes: 'output/*.md'
}


}