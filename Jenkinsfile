pipeline {
    agent { label'JDK_8'}
    triggers{ pollSCM ( '* * * * *' ) }
    parameters { 
        choice(name: 'MAVEN_GOAL', choices:['package', 'install', 'clean'], description:'mavengoal')
    }
    stages{
        stage ('scm') {
            steps{
                git url: 'https://github.com/raviteja811811/spring-petclinic-R.git',
                    branch : 'try'
            }
        }
        stage('Sonar analysis') {
            steps {
                sh 'mvn clean package sonar:sonar -Dsonar.organization=spring-petclinic-R'
            }
        }
        stage ('packege') {
            tools {
                jdk 'JDK_11_UBUNTU'
            }
            steps {
                sh "mvn ${parmes.MAVEN_GOAL}"
            }
        }
        stage ('post build'){
            steps{
                archiveArtifacts artifacts: '**/target/springpetclinic.war',
                                 onlyIfSuccessful: true
                junit testResults : '**/surefire-reports/test-*.xml'
            }
        }
    }
}