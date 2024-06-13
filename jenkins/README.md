export KUBECONFIG=/etc/kubernetes/admin.conf

## Repo Setup

helm repo list
helm repo add jenkinsci https://charts.jenkins.io/
helm repo update

## install jenkins

helm install my-jenkins jenkinsci/jenkins --version 5.1.26

helm install devops-jenkins jenkinsci/jenkins --version 5.1.26 -f values_jenkins.yaml 
-f values_main.yaml -f values_plugins.yaml \
-f values_jcasc_main.yaml -f values_jcasc_credentials.yaml -f values_jcasc_system_config.yaml \
-f values_jcasc_sonar.yaml -f values_jcasc_jira.yaml -f values_jcasc_gitlab.yaml -f values_jcasc_emails.yaml \
-f values_jcasc_nodes.yaml -f values_jcasc_tools.yaml -f values_jcasc_views.yaml \
-n devops-tools


helm upgrade devops-jenkins jenkinsci/jenkins -f values_jenkins.yaml \
-f values_main.yaml -f values_plugins.yaml \
-f values_jcasc_main.yaml -f values_jcasc_credentials.yaml -f values_jcasc_system_config.yaml \
-f values_jcasc_sonar.yaml -f values_jcasc_jira.yaml -f values_jcasc_gitlab.yaml -f values_jcasc_emails.yaml \
-f values_jcasc_nodes.yaml -f values_jcasc_tools.yaml -f values_jcasc_views.yaml \
 --recreate-pods \
-n devops-tools



helm uninstall vision-jenkins jenkins/jenkins -n devops-tools


kubectl get all  --all-namespaces
kubectl get pods --all-namespaces

kubectl get pods  -n devops-tools
kubectl get all  -n devops-tools

kubectl -n devops-tools describe pod devops-jenkins-0
kubectl -n devops-tools logs devops-jenkins-0
kubectl -n devops-tools logs devops-jenkins-0 -f


