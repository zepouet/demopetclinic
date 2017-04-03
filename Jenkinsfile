def host = "http://cu-tomcat:8080" 
def username = "johndoe"
def password = "abc2015"
def appname

stage "Init SCM"
node {
    properties([disableConcurrentBuilds()])
    checkout scm
    committer = gitLog.committer().replaceAll(/\s/,"").toLowerCase().take(5)
    emailCommiter = gitLog.email()
    branch = env.BRANCH_NAME.replaceAll(/.*\//,"").toLowerCase().take(8)
    appname = "${branch}"
}

stage "Reset env"
node {
    cloudunit(host, username, password, """
        rm-app --name pc-${appname} --scriptUsage
    """)
}