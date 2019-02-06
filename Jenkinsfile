pipeline {

                agent any
                stages {
                    stage ('Checkout') {
                       // Git checkout the branch.
                               echo "This is checkout"

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