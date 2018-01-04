node {
  def project = 'devops-preprod'
  def appName = 'gerua'
  def imageTag = "gcr.io/${project}/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"

  checkout scm

  stage 'Build image'
  sh("docker build -t ${imageTag} .")

  stage 'Run Go tests'
  sh("docker run ${imageTag} go test")

  stage 'Push image to registry'
  sh("gcloud docker -- push ${imageTag}")

  stage "Deploy Application"
  switch (env.BRANCH_NAME) {
    // Roll out to canary environment
    case "canary":
        // Change deployed image in canary to the one we just built
        sh("echo project ${project} , appName ${appName} ")
        break

    // Roll out to production
    case "master":
        // Change deployed image in canary to the one we just built
        sh("echo project ${project} , appName ${appName} ")
        break
  }
}
