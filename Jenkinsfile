pipeline {

    node('cloud-build && linux') {
        timeout(time: 3, unit: 'HOURS') {

            try {

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
                       releaseBuild = branch != "develop" &&
                                                      !branch.startsWith("feature/") &&
                                                      !branch.startsWith("hotfix/") &&
                                                      !branch.startsWith("release/")
                                       withMaven(maven: mavenToolName, globalMavenSettingsConfig: mavenSettings, jdk: jdkToolName,
                                               mavenOpts: "-XX:+TieredCompilation -XX:TieredStopAtLevel=1 -Xmx64m " +
                                                       "-Dorg.slf4j.simpleLogger.defaultLogLevel=ERROR " +
                                                       "-Dorg.slf4j.simpleLogger.log.org.apache.maven.plugins.help=INFO") {
                                           version = sh(script: "mvn --batch-mode help:evaluate -Dexpression=project.version | grep -v '^\\[' | grep -v 'JenkinsMavenEventSpy' | tail -1",
                                                               returnStdout: true).trim()
                                       }

                                       if (releaseBuild && version.endsWith('-SNAPSHOT')) {
                                           error("Version in release branch should not be a SNAPSHOT (is ${version}).")
                                       }
                                       else if (!releaseBuild && !version.endsWith('-SNAPSHOT')) {
                                           error("Version in non-release branch should be a SNAPSHOT (is ${version}).")
                                       }

                                       // Make sure Docker daemon is running and accessible, otherwise later Docker stages will fail.
                                       sh 'docker version'
                    }
                    stage ('Build and Test') {
                               // Get the git commit manually as SCM-specific variables are currently unavailable inside a Pipeline script.
                               def gitCommit = currentBuild.description

                               // If this is a release build (on master) make sure the artefacts get deployed (used by downstream projects such as TDE).
                               final targetPhase = (releaseBuild) ? "deploy -DdeployAtEnd" : "install"

                               // Main part of build included deployment of artifacts.
                               withMaven(maven: mavenToolName, globalMavenSettingsConfig: mavenSettings, jdk: jdkToolName, mavenOpts: mavenJvmOptions) {
                                   sh "mvn ${mavenOptions} clean ${targetPhase} \
                                           -DSVN_REVISION=${gitCommit} -DSVN_URL=${scmUrl} -Dscm.url=${scmUrl} -Dscm.revision=${gitCommit} \
                                           -DJOB_NAME=${env.JOB_NAME} -DBUILD_NUMBER=${env.BUILD_NUMBER}"
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
            catch (e) {
             //failure/success email etc ...
            }

        }


     }



}