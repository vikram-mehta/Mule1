node {
  
    def server = Artifactory.newServer url: "${params.arti_url}", username: "${params.arti_user}", password: "${params.arti_password}"
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo = Artifactory.newBuildInfo()
    stage ('Clone') {
        git url: 'https://github.com/vikram-mehta/Mule1.git'
    }

    stage ('Artifactory configuration') {
        // Obtain an Artifactory server instance, defined in Jenkins --> Manage:
        rtMaven.tool = 'M3' // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'mule-repo-local', snapshotRepo: 'mule-repo-local', server: server
        rtMaven.resolver releaseRepo: 'mule-repo', snapshotRepo: 'mule-repo', server: server
        rtMaven.deployer.deployArtifacts = false // Disable artifacts deployment during Maven run

    }
 
    stage ('Test') {
        //rtMaven.run pom: 'pom.xml', goals: 'clean test'
    }
        
    stage ('Install') {
        rtMaven.run pom: 'pom.xml', goals: 'clean install mule:deploy -Dusername=${params.anyp_user} -Dpassword=${params.anyp_password}', buildInfo: buildInfo
    }
 
    stage ('Deploy') {
        rtMaven.deployer.deployArtifacts buildInfo
    }
        
    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
