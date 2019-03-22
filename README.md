# Develop and Build
Build:

```
cd sb-app-docker
mvn install dockerfile:build
docker sb-app-docker tag 172.30.1.1:5000/minishift-operator/sb-app-docker
docker push 172.30.1.1:5000/minishift-operator/sb-app-docker
```

# Deploy
## S2I
S2I app:
```
oc new-app https://github.com/dav1dli/minishift-operator.git \
     --context-dir=gs-spring-boot-docker \           
     --docker-image="registry.redhat.io/redhat-openjdk-18/openjdk18-openshift"
oc expose svc/minishift-operator
```

## YAML
Install: `oc create -f yaml/sb-example.yaml`

Uninstall: `oc delete -f yaml/sb-example.yaml`

## Helm
Install helm chart: `helm install ./helm/sb-app -n sb-app`

Uninstall: `helm delete --purge sb-app`
