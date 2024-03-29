node {
    def app
    stage('Clone repository') {
        checkout scm
    }
    stage('Build image') {
        if (env.BRANCH_NAME == 'dev') {
            app = docker.build("ana-kostadinovska/kiii-jenkins")
        } else {
            echo "Changes are not on the dev branch"
        }
    }
    stage('Push image') {
        if (env.BRANCH_NAME == 'dev') {
            docker.withRegistry('https://registry.hub.docker.com', 'ak-github') {
                app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                app.push("${env.BRANCH_NAME}-latest")
                // signal the orchestrator that there is a new version
            }
        } else {
            echo "Changes are not on the 'dev' branch"
        }
    }
}
