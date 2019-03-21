# operator-test
Build:
cd sb-app-docker
mvn install dockerfile:build
docker tag
docker push

Install helm chart:
helm install ./helm/sb-app -n sb-app
Uninstall: helm delete --purge sb-app
