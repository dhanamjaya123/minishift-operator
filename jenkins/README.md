# Operator
## Bootstrap
Basic command to create a new operator project from a chart: `operator-sdk new <project-name> --type=helm --helm-chart=<chart>`

The result is a directory with a skeleton operator file structure.

For example: `operator-sdk new app-operator \
   --type=helm --helm-chart=./sb-example/helm/sb-app`

## Build and run
Deploy Custom Resource Definition: `oc create -f deploy/crds/charts_v1alpha1_sbapp_crd.yaml --as=system:admin`

Build operator image:
```
operator-sdk build quay.io/<user>/app-operator:v0.0.1
docker push quay.io/<user>/app-operator:v0.0.1
```

Kubernetes deployment manifests are generated in deploy/operator.yaml. The deployment image in this file needs to be modified from the placeholder REPLACE_IMAGE to the previously built image. To do this run:
`sed -i 's|REPLACE_IMAGE|quay.io/<user>/app-operator:v0.0.1|g' deploy/operator.yaml`

Deploy the app-operator:
```
oc create -f deploy/service_account.yaml
oc create -f deploy/role.yaml --as=system:admin
oc create -f deploy/role_binding.yaml --as=system:admin
oc create -f deploy/operator.yaml
```
Verify that the app-operator is up and running: `oc get deployment`


Deploy the App custom resource: `oc apply -f deploy/crds/charts_v1alpha1_sbapp_cr.yaml`

## Cleanup
Clean up the resources:
```
oc delete -f deploy/crds/charts_v1alpha1_sbapp_cr.yaml
oc delete -f deploy/operator.yaml
oc delete -f deploy/role_binding.yaml
oc delete -f deploy/role.yaml
oc delete -f deploy/service_account.yaml
oc delete -f deploy/crds/charts_v1alpha1_sbapp_cr.yaml
```
