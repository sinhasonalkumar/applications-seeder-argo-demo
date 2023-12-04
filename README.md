mk delete --all

brew upgrade kubectl

brew link --overwrite kubernetes-cli

brew upgrade argocd

mk start start --memory=8192 --cpus=6 --driver=virtualbox

mk addons enable volumesnapshots

mk addons enable metallb

mk ip

mk addons configure metallb

cd /Users/sonalkumarsinha/sonal/workspace/infraDevWork/istio

./installlIstio.sh

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubens argocd

k get secret argocd-initial-admin-secret -n argocd -o yaml | yq .data.password | base64 -d

k edit svc argocd-server

# Change svc of type to LoadBalancer

k get svc

cd /Users/sonalkumarsinha/sonal/workspace/git/github/argo/argo-cd-demo/helm-charts-argo-demo
# Do helm chart changes
yq -i '.version="0.2.0"' ./micro-service-helm-chart/Chart.yaml

git status
git add .
git commit -m "feat: demo argocd helm chart release"
git push -u origin main
git tag 0.2.0
git push origin 0.2.0

helm package micro-service-helm-chart -d ../micro-service-helm-repository-argo-demo/micro-service-helm-chart
cd ../micro-service-helm-repository-argo-demo/micro-service-helm-chart
ls
cd ../
helm repo index .

git status
git add .
git commit -m "feat: demo argocd helmc chart release"
git push -u origin main

----

---
to add helm repo to local
---
 helm repo add micro-service-helm-repository-argo-demo https://sinhasonalkumar.github.io/micro-service-helm-repository-argo-demo/

 helm repo list

 helm search repo micro-service-helm-repository-argo-demo

 helm repo update micro-service-helm-repository-argo-demo

 helm search repo micro-service-helm-repository-argo-demo

---

cd /Users/sonalkumarsinha/sonal/workspace/git/github/argo/argo-cd-demo/applications-k8s-state-argo-demo

cd sp-micro-service-argo-demo/manifests/dev

# update helm chart version in Chart.yaml

helm dependency update .

# to the upgrade for rest of env in their respective directory

cd -

git status
git add .
git commit -m "feat: demo argocd helmc chart release"
git push -u origin main

----


cd /Users/sonalkumarsinha/sonal/workspace/git/github/argo/argo-cd-demo/base-app-networking-k8s-state-argo-demo

cd base-app-networking/manifests/dev

# update helm chart version in Chart.yaml

helm dependency update .

# to the upgrade for rest of env in their respective directory

cd -

git status
git add .
git commit -m "feat: demo argocd helmc chart release"
git push -u origin main

----

k get secret argocd-initial-admin-secret -n argocd -o yaml | yq .data.password | base64 -d
k get svc argocd-server

argocd login 192.168.59.144
# admin/Lma4sbIJizPpAlgx
argocd app list

https://192.168.59.144/

# admin/Lma4sbIJizPpAlgx



------


# Onborad applications

cd /Users/sonalkumarsinha/sonal/workspace/git/github/argo/argo-cd-demo/applications-seeder-argo-demo

k apply -f argocd/infrastructure/application.yaml
k apply -f argocd/applications/application.yaml

----



----

# Onboarded Based App Networking Stack Repo
cd /Users/sonalkumarsinha/sonal/workspace/git/github/argo/argo-cd-demo/onboarded-base-app-networking-stack-argo-demo
ls
ls base-app-networking
# Onboarded Applications Repo
cd /Users/sonalkumarsinha/sonal/workspace/git/github/argo/argo-cd-demo/onboarded-applications-argo-demo
ls
ls sp-micro-service-argo-demo
----

https://hub.docker.com/repository/docker/sinhasonalkumar/sp-micro-service-argo-demo/general

cd /Users/sonalkumarsinha/sonal/workspace/git/github/argo/argo-cd-demo/sp-micro-service-argo-demo
git checkout -b feature/f1
git add .
git commit -m "feat: code change"
git push -u origin feature/f1

# raise pull request
# code review
# merge PR
# go to action to watch build code change with container image push

---

cd /Users/sonalkumarsinha/sonal/workspace/git/github/argo/argo-cd-demo/applications-k8s-state-argo-demo

gh auth login

git pull

git checkout -b feature/promoteToDev


yq '.micro-service-helm-chart.imageVersion' ./sp-micro-service-argo-demo/manifests/dev/values.yaml
yq -i '.micro-service-helm-chart.imageVersion="1.2.0"' ./sp-micro-service-argo-demo/manifests/dev/values.yaml
yq '.micro-service-helm-chart.imageVersion' ./sp-micro-service-argo-demo/manifests/dev/values.yaml

git diff

git add .

git commit -m "feat:promoting 1.2.0 to dev"

git push -u origin feature/promoteToDev

gh pr create --title "feat:promoting 1.2.0 to dev" --body "feat:promoting 1.2.0 to dev"

gh pr merge  -d -s

argocd app sync dev-sp-micro-service-argo-demo

---


---

k get svc istio-ingressgateway -n istio-system

### get external IP and update to etc hosts file

vi /etc/hosts

192.168.59.143 api.mycompany.com
192.168.59.143 mgmt-api.mycompany.com

192.168.59.143 api-dev.mycompany.com
192.168.59.143 mgmt-api-dev.mycompany.com

192.168.59.143 api-qa.mycompany.com
192.168.59.143 mgmt-api-qa.mycompany.com

192.168.59.143 api-stage.mycompany.com
192.168.59.143 mgmt-api-stage.mycompany.com

192.168.59.143 api-preview.mycompany.com
192.168.59.143 mgmt-api-preview.mycompany.com

---

http://api-dev.mycompany.com/ds/v1/products

http://api-dev.mycompany.com/ds/v1/canary

http://mgmt-api-dev.mycompany.com/ds/actuator/health

http://mgmt-api-dev.mycompany.com/ds/api-docs/v1/swagger-ui/index.html


---

http://api-qa.mycompany.com/ds/v1/products

http://api-qa.mycompany.com/ds/v1/canary

http://mgmt-api-qa.mycompany.com/ds/actuator/health

http://mgmt-api-qa.mycompany.com/ds/api-docs/v1/swagger-ui/index.html


---

http://api-stage.mycompany.com/ds/v1/products

http://api-stage.mycompany.com/ds/v1/canary

http://mgmt-api-stage.mycompany.com/ds/actuator/health

http://mgmt-api-stage.mycompany.com/ds/api-docs/v1/swagger-ui/index.html

----

http://api.mycompany.com/ds/v1/products

http://api.mycompany.com/ds/v1/canary

http://mgmt-api.mycompany.com/ds/actuator/health

http://mgmt-api.mycompany.com/ds/api-docs/v1/swagger-ui/index.html



----
If One K8s Cluster per BU

Repo Across OU
 - helm-charts-argo-demo
 - base-app-networking-helm-repository-argo-demo
 - micro-service-helm-repository-argo-demo

Repo Per K8s Cluster
 - applications-seeder-argo-demo
 - onboarded-base-app-networking-stack-argo-demo
 - base-app-networking-k8s-state-argo-demo
 - onboarded-applications-argo-demo

Repo Per BU
 - applications-k8s-state-argo-demo

Repo Per MicroService
 - sp-micro-service-argo-demo
 - sp-micro-service-config-argo-demo


















