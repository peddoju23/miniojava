node {
    def mvnHome
    stage('Preparation') { 
        git 'https://github.com/jglick/simple-maven-project-with-tests.git'
        mvnHome = tool 'M3'
    }
    stage('Build') {
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
    stage('Results') {
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'target/*.jar'
    }
    stage('Build Image') {
        sh 'docker build -t myprojectharbor/myprojectminio .''
    }
    stage('Docker push'){
        sh 'docker login -u dockerhuduson -p'
        sh 'docker push myproject/myprojectminio'
    }
    }
