pipeline {

                stages {
                    stage ('Checkout') {
                       // Git checkout the branch.
                               checkout scm

                               // Set build description to commit hash for convenience.
                               currentBuild.description = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()

                               // Make sure that committer and author are included in notification emails.
                               notificationRecipients.add(sh(returnStdout: true, script: 'git --no-pager log -1 --pretty=format:"%ae"').trim())
                               notificationRecipients.add(sh(returnStdout: true, script: 'git --no-pager log -1 --pretty=format:"%ce"').trim())
                               notificationRecipients.unique()
                               print "Notification Recipients: " + notificationRecipients.join(" ")
                    }

                    stage ('Verify') {
                               steps {
                                       sh '''
                                           echo "PATH = ${PATH}"
                                           echo "M2_HOME = ${M2_HOME}"
                                       '''
                                   }

                    }
                    stage ('Build and Test') {
                               steps {
                                             sh '''
                                                   echo "PATH = ${PATH}"
                                                   echo "M2_HOME = ${M2_HOME}"
                                               '''
                                           }


                    }
                    stage ('Docker Build') {

                                steps {
                                    sh '''
                                        echo "PATH = ${PATH}"
                                        echo "M2_HOME = ${M2_HOME}"
                                    '''
                                }
                    }
                    stage ('Docker Push') {

                                steps {
                                    sh '''
                                        echo "PATH = ${PATH}"
                                        echo "M2_HOME = ${M2_HOME}"
                                    '''
                                }
                     }
                }

}