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

stage "Build project"
node {
    sh 'mvn clean deploy -DskipTests'
    archiveArtifacts artifacts: 'target/spring-petclinic-1.5.1.jar'
}

stage "Create Application"
node {
    cloudunit(host, username, password, """
        create-app --name pc-${appname} --type fatjar
        add-jvm-options " -Dspring.profiles.active=mysql "
    """)
}

stage "Create Mysql"
node {
    cloudunit(host, username, password, """
        use pc-${appname}
        add-module --name mysql-5-5
    """)
}

stage "Deploy"
node {
    cloudunit(host, username, password, """
        use pc-${appname}
        deploy --path target/spring-petclinic-1.5.1.jar --openBrowser false
    """)
}


