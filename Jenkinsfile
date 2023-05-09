node {

    //URL to Github repository https://github.com/<owner>/<repo>
    def GITHUB_REPO_URL="https://github.com/jenkins-23/pipeline-test"
    def GITHUB_BRANCH="main"
    
    //Retrieve ENV ID values for DEV and QA from "Download attributes from API" json file
    def VELOCITY_ENV_ID_DEV="abf22e91-6813-45bd-9dc4-47d3d6d7b85c"
    def VELOCITY_ENV_ID_QA="f5a26083-3128-4f78-a928-ce5074de9e3e"
    def VELOCITY_ENV_ID_PROD="e36dbb12-60ba-4247-94bb-828af3d4564b"

    //VELOCITY_APP_NAME must match your Velocity pipeline application name
    def VELOCITY_APP_NAME="pipeline_test"
    def VELOCITY_APP_ID="5155a71f-7a62-4b44-b76f-f655bc7d5e06"

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
