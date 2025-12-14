pipeline{
    agent {
        label 'java_from_python_node'
    }

    stages{

        stag('prod-pull'){
            steps {
                git branch: 'main', url: 'https://github.com/sprint-projects/sprint-petclinic.git'
            }
        }


        stage('prod-shift-left'){
            parallel{
                stag('build'){
                    steps {
                        sh 'mvn clean package -Dmaven.gitcommitid.skip=true -Dcheckstyle.skip -Dmaven.test.skip=true'
                    }
                    post{
                        sucess{
                            archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                        }
                    }
                }
                stag('test'){
                    steps {
                        sh 'mvn clean test -Dmaven.test.failure.ignore=true -Dmaven.gitcommitid.skip=true -Dcheckstyle.skip -Dmaven.test.skip=true'
                    }
                    post{
                        sucess{
                            junit 'target/surefire-reports/*.xml'
                        }
                    }
                }

            }
        }






        stage('it'){
            parallel{
                stage('pro-run'){
                    steps{
                        sh 'java -jar target/*.jar'
                    }
                }
                stage('test-script'){
                    stage('pull'){
                        steps {
                            git branch: 'main', url: 'https://github.com/hishiry/for_jenkins_agent_java.git'
                        }
                    }
                    stage("shift-left"){
                        parallel{
                            stage('build'){
                                steps{
                                    sh 'mvn clean package -Dmaven.test.skip=true'
                                }
                                post{
                                    sucess{
                                        archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                                    }
                                }
                            }

                            stage('test'){
                                steps{
                                    sh 'mvn clean test -Dmaven.test.failure.ignore=true'
                                }
                                post{
                                    sucess{
                                        junit 'target/surfire-reports/*.xml'
                                    }
                                }
                            }
                        }
                    }
                }
            }

        }

    }
}

