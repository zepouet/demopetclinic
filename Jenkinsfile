def host = "http://192.168.50.1:8080" 
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

stage "Create Backend"
node {
    cloudunit(host, username, password, """
        create-app --name pc-${appname} --type tomcat-8
    """)
}