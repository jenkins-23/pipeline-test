node {

    //URL to Github repository https://github.com/<owner>/<repo>
    def GITHUB_REPO_URL="https://github.com/jenkins-23/pipeline-test"
    def GITHUB_BRANCH="main"
    
    //Retrieve ENV ID values for DEV and QA from "Download attributes from API" json file
    def VELOCITY_ENV_ID_DEV="f72392bf-eac7-4b9f-b36e-65717da9c626"
    def VELOCITY_ENV_ID_QA="5109b5e5-91d5-43a3-a620-5d415fa5803e"
    def VELOCITY_ENV_ID_PROD="9914bf57-c046-41d5-a3f1-f23ed4346609"

    //VELOCITY_APP_NAME must match your Velocity pipeline application name
    def VELOCITY_APP_NAME="pipeline_test"
    //def VELOCITY_APP_ID="df8ffa40-cfdc-42a8-acfd-c87ec4f91b7e"

    //Do not change below this line.
    def GIT_COMMIT
    
    stage ('cloning the repository'){
      currentBuild.displayName = "2.1.1.${BUILD_NUMBER}"
      majorVersion="${BUILD_NUMBER}"
      def scm = git branch: "${GITHUB_BRANCH}", url: "${GITHUB_REPO_URL}"
      GIT_COMMIT = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
      echo "GIT_COMMIT=${GIT_COMMIT}"
    }
    stage ("Build") {
        echo "Building ${VELOCITY_APP_NAME} (Build:${currentBuild.displayName}, GIT_COMMIT:${GIT_COMMIT})"
        step($class: 'UploadBuild', 
           tenantId: "5ade13625558f2c6688d15ce",
           revision: "${GIT_COMMIT}",
           appName: "${VELOCITY_APP_NAME}",
	   appID: "${VELOCITY_APP_ID}",
           versionName:"${currentBuild.displayName}",
           requestor: "admin", id: "${currentBuild.displayName}"
        )
    }
   stage ("Deploy to DEV") {
    sleep 10
    step([$class: 'UploadDeployment',
          tenantId: "5ade13625558f2c6688d15ce",
          versionName: "${currentBuild.displayName}",
          versionExtId: "${currentBuild.displayName}",
          type: 'Jenkins(Legacy)',
          environmentId: "${VELOCITY_ENV_ID_DEV}",
          environmentName: 'DEV',
          appName: "${VELOCITY_APP_NAME}",
	  appID: "${VELOCITY_APP_ID}",
          description: '[Description ex: Terraform Deployment]',
          initiator: "admin",
		  result: 'true'
      ])
   }
   stage ("Deploy to QA") {
    sleep 10
    step([$class: 'UploadDeployment',
          tenantId: "5ade13625558f2c6688d15ce",
          versionName: "${currentBuild.displayName}",
          versionExtId: "${currentBuild.displayName}",
          type: 'Jenkins(Legacy)',
          environmentId: "${VELOCITY_ENV_ID_QA}",
          environmentName: 'QA',
          appName: "${VELOCITY_APP_NAME}",
	  appID: "${VELOCITY_APP_ID}",
          description: '[Description ex: Terraform Deployment]',
          initiator: "admin",
		  result: 'true'
      ])
   }
   stage ("Deploy to PROD") {
    sleep 10
    step([$class: 'UploadDeployment',
          tenantId: "5ade13625558f2c6688d15ce",
          versionName: "${currentBuild.displayName}",
          versionExtId: "${currentBuild.displayName}",
          type: 'Jenkins(Legacy)',
          environmentId: "${VELOCITY_ENV_ID_QA}",
          environmentName: 'QA',
          appName: "${VELOCITY_APP_NAME}",
	  appID: "${VELOCITY_APP_ID}",
          description: '[Description ex: Terraform Deployment]',
          initiator: "admin",
		  result: 'true'
      ])
   }
}
