node {
  def project = 'gdg-demo-165104'
  def appName = 'kube-sample-app'
  def imageTag = "asia.gcr.io/${project}/${appName}:v${env.BUILD_NUMBER}"
  def NAMESPACE = 'frontend'

  checkout scm

  stage 'Build image'
  sh("docker build -t ${imageTag} .")

  stage 'Push image to registry'
  sh("gcloud docker push ${imageTag}")

  stage "Deploy Application"
  // Roll out to production
  sh("sed -i.bak 's#asia.gcr.io/${project}/${appName}:v1#${imageTag}#' ./k8s/dc.yaml")
  sh("kubectl --namespace=${NAMESPACE} apply -f ./k8s/dc.yaml")
  
}