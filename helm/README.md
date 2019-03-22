# Helm
Helm/Tiller can run on OpenShift but it is unsupported by Red Hat. Helm charts can be used to create K8S Operators. In the scope of this document Helm Tiller server can be installed for validation purposes only and Helm client is used to manipulate local charts. See [Getting started with Helm on OpenShift](https://blog.openshift.com/getting-started-helm-openshift/) for more details about the installation.
```
oc new-project tiller
export TILLER_NAMESPACE=tiller
helm init --client-only
oc process -f https://github.com/openshift/origin/raw/master/examples/helm/tiller-template.yaml -p TILLER_NAMESPACE="${TILLER_NAMESPACE}" -p HELM_VERSION=v2.13.0 | oc create -f -
oc rollout status deployment tiller
oc new-project helm-demo
oc policy add-role-to-user edit "system:serviceaccount:${TILLER_NAMESPACE}:tiller"
helm install helm/sb-app -n sb-app
```
