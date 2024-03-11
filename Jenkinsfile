node {
    def server
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
    stage ('Clone') {
        git url: 'https://github.com/ganavikumar/cloud-test.git'
    }
    stage ('Artifactory configuration') {
        // Obtain an Artifactory server instance, defined in Jenkins --> Manage Jenkins --> Configure System:
        server = Artifactory.server 'maven-cloud'
        // Tool name from Jenkins configuration
        rtMaven.tool = 'maven-test'
        rtMaven.deployer releaseRepo: 'clous-libs-release-local', snapshotRepo: 'clous-libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'clous-libs-release', snapshotRepo: 'clous-libs-snapshot', server: server
        buildInfo = Artifactory.newBuildInfo()
    }
    stage ('Exec Maven') {
        rtMaven.run pom: 'maven-examples/maven-example/pom.xml', goals: 'clean install', buildInfo: buildInfo
    }
    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
