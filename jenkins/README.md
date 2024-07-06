

## Jenkins Deployment

### Jenkins deployment with helm
export KUBECONFIG=/etc/kubernetes/admin.conf
> kubectl create namespace devops

------------

#### helm repo Installation
`helm repo list`
`helm repo add jenkinsci https://charts.jenkins.io/`
`helm repo update`

Examples:
> helm install devops-jenkins jenkinsci/jenkins --version 5.3.1 -n devops
> helm uninstall devops-jenkins jenkins/jenkins -n devops


#### create pv and storage class
`kubectl apply -f jenkins-storage-class.yaml`
`kubectl apply -f jenkins-vol.yaml`


#### Installation & Upgrade DevOps Jenkins
`helm install devops-jenkins jenkinsci/jenkins --version 5.3.1 \
-f values_jenkins_devops.yaml -f values_jcasc_roles_devops.yaml -f values_jcasc_views_devops.yaml \
-f values_main.yaml -f values_plugins.yaml -f values_jcasc_main.yaml -f values_jcasc_credentials.yaml \
-f values_jcasc_system_config.yaml -f values_jcasc_tools.yaml -f values_jcasc_nodes.yaml \
-f values_jcasc_sonar.yaml -f values_jcasc_jira.yaml -f values_jcasc_gitlab.yaml -f values_jcasc_emails.yaml \
-n devops`


##### Upgrade without plugins installation
`helm upgrade devops-jenkins jenkinsci/jenkins 
-f values_jenkins_devops.yaml -f values_jcasc_roles_devops.yaml -f values_jcasc_views_devops.yaml \
-f values_main.yaml -f values_plugins.yaml -f values_jcasc_main.yaml -f values_jcasc_credentials.yaml \
-f values_jcasc_system_config.yaml -f values_jcasc_tools.yaml -f values_jcasc_nodes.yaml \
-f values_jcasc_sonar.yaml -f values_jcasc_jira.yaml -f values_jcasc_gitlab.yaml -f values_jcasc_emails.yaml \
--atomic --timeout 15m --debug --recreate-pods 
-n devops`

##### Upgrade with plugins installation
`helm upgrade devops-jenkins jenkinsci/jenkins 
-f values_jenkins_devops.yaml -f values_jcasc_roles_devops.yaml -f values_jcasc_views_devops.yaml \
-f values_main.yaml -f values_plugins.yaml -f values_jcasc_main.yaml -f values_jcasc_credentials.yaml \
-f values_jcasc_system_config.yaml -f values_jcasc_tools.yaml -f values_jcasc_nodes.yaml \
-f values_jcasc_sonar.yaml -f values_jcasc_jira.yaml -f values_jcasc_gitlab.yaml -f values_jcasc_emails.yaml \
--atomic --timeout 20m --debug --recreate-pods controller.initializeOnce=false 
-n devops`


##### Rollback upgrade
`helm history devops-jenkins`
`helm rollback devops-jenkins 1 -n devops`

> Note: 1 is a release number


##### Uninstall Jenkins
`helm uninstall devops-jenkins jenkinsci/jenkins -n devops`


------------


#### Commands to verify deployments

helm ls -a -n devops
helm list --all-namespaces

kubectl get all  --all-namespaces
kubectl get pods --all-namespaces

kubectl get pods  -n devops
kubectl get all  -n devops
kubectl get all,pv,pvc  -n devops

kubectl -n devops describe pod devops-jenkins-0
kubectl -n devops logs devops-jenkins-0
kubectl -n devops logs devops-jenkins-0 -f

