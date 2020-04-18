# TRABALHO DEVOPS - SETREM

Maria Gisele Flores e Wagner Silveira

Neste relatório é transcrito o processo de recriação do Pipeline de CI/CD usado em exemplo
com o Jenkins. Sendo este pipeline usado como guia de requisitos que seram atendidos pelo novo.
A ferramenta escolhida foi o Drone por ser open source, contemplar uma vasta documentação e comunidade ativa. Com simplicidade na instalação e configuração.

### Requisitos técnicos avaliados

- O Drone permite configurar pipelines com arquivos no formato YMAL.
- Cada etapa é executrada isoladamente em um container Docker.
- Podendo ser executado em diversas sistemas operacionais.
- Integração com múltiplos CMS's (GitHub, GitLab, BitBucket, etc.).
- Vasta gama de plugins para integrações com AWS, GCP, e com a Docker.

### Aplicação

A aplicação entregue pelo pipeline é a disponibilizada em aula (programa Hello-World).

### Etapas

Utilizando o GCP com cluster Kubernets, o Drone foi integrado ao GitHub e no pipeline foram definidas as etapas: - Test - Build - Publish - Deploy - Notify

### Instruções

As seguintes instruções devem ser seguidas para executar o pipeline:

```sh
wagnerdevel@cloudshell:~ (devops-cc)$ history
    1  gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project devops-cc
    2  helm
    3  helm version
    4  kubectl -n kube-system create serviceaccount tiller
    5  kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
    6  helm init --service-account tiller
    7  kubectl create secret generic drone-server-secrets --namespace=default --from-literal=clientSecret="XXXXXXX"
    8  helm install --name drone-release stable/drone
    9  helm upgrade drone --reuse-values --set 'service.type=NodePort' --set 'service.nodePort=32000' --set 'sourceControl.provider=github' --set 'sourceControl.github.clientID=XXXXXXX' --set 'sourceControl.secret=drone-server-secrets' stable/drone
   10  helm install --name drone --namespace drone stable/drone
   11  helm upgrade drone --reuse-values --set 'service.type=NodePort' --set 'service.nodePort=32000' --set 'sourceControl.provider=github' --set 'sourceControl.github.clientID=XXXXXXX' --set 'sourceControl.secret=drone-server-secrets' stable/drone
   12  kubectl get nodes -o wide
   13  helm upgrade drone-release --reuse-values --set 'server.host=http://XXXXXXX:32000' stable/drone
   14  helm upgrade drone --reuse-values --set 'server.host=http://XXXXXXX:32000' stable/drone
   15  helm ls
   22  kubectl -n kube-system create serviceaccount tiller
   23  gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project devops-cc
   24  kubectl -n kube-system create serviceaccount tiller
   25  kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
   26  helm init --service-account tiller
   27  kubectl create secret generic drone-server-secrets --namespace=default --from-literal=clientSecret="XXXXXXX"
   28  helm install --name drone-release stable/drone
   29  helm upgrade drone-release --reuse-values --set 'service.type=NodePort' --set 'service.nodePort=32000' --set 'sourceControl.provider=github' --set 'sourceControl.github.clientID=XXXXXXX' --set 'sourceControl.secret=drone-server-secrets' stable/drone
   30  kubectl get nodes -o wide
   31  helm upgrade drone-release --reuse-values --set 'server.host=http://XXXXXXX:32000' stable/drone
   32  helm upgrade drone-release --reuse-values --set 'service.type=LoadBalancer' --set 'service.nodePort=32000' --set 'sourceControl.provider=github' --set 'sourceControl.github.clientID=XXXXXXX' --set 'sourceControl.secret=drone-server-secrets' stable/drone
   33  kubectl get nodes -o wide
   34  helm upgrade drone-release --reuse-values --set 'service.type=LoadBalancer' --set 'service.nodePort=32000' --set 'sourceControl.provider=github' --set 'sourceControl.github.clientID=XXXXXXX' --set 'sourceControl.secret=drone-server-secrets' stable/drone
   35  helm upgrade drone-release --reuse-values --set 'server.host=http://XXXXXXX:32000' stable/drone
   36  export SERVICE_IP=$(kubectl get svc drone-release-drone -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
   37  echo http://$SERVICE_IP/
   38  ls
   39  rm main*
   40  rm Dockerfile
   41  kubectl get secret drone-token-sn77f -o jsonpath='{.data.ca\.crt}' && echo
   42  ls -l
   43  ll
   44  echo http://$SERVICE_IP/
   45  gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project devops-cc
   46  echo http://$SERVICE_IP/
   47  kubectl get secret drone-token-sn77f -o jsonpath='{.data.ca\.crt}' && echo
   48  kubectl get secret drone-token-sn77f -o jsonpath='{.data.token}' | base64 --decode && echo
   49  helm ls
   50  kubectl get secrets
   51  kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.ca\.crt}' && echo
   52  kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.token}' | base64 --decode && echo
   53  kubectl config view -o jsonpath='{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
   54  kubectl get secrets
   55  echo http://$SERVICE_IP/
   56  kubectl describe
   57  kubectl api-versions
   58  kubectl logs -l app=catalog-catalog-apiserver -n catalog -c apiserver
   59  kubectl get apiservices
   60  kubectl get clusterservicebrokers
   61  kubectl describe apiservice
   62  kubectl get apiservices
   63  kubectl describe apiservice
   64  kubectl api-versions
   65  kubectl logs -l app=catalog-catalog-apiserver -n catalog -c apiserver
   66  kubectl config view -o jsonpath='{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
   67  kubectl -n tiller get secret $(kubectl -n tiller get sa tiller -o jsonpath='{.secrets[].name}{"\n"}') -o jsonpath="{.data.token}" | base64 -D
   68  kubectl config view -o jsonpath='{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
   69  kubectl -n drone-release get secret $(kubectl -n drone-release get sa tiller -o jsonpath='{.secrets[].name}{"\n"}') -o jsonpath="{.data.token}" | base64 -D
   70  kubectl config view -o jsonpath='{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
   71  helm ls
   72  kubectl -n drone-release get secret $(kubectl -n drone-release get sa drone-release -o jsonpath='{.secrets[].name}{"\n"}') -o jsonpath="{.data.token}" | base64 -Ddrop
   73  kubectl -n drone-release get secret $(kubectl -n drone-release get sa drone-release -o jsonpath='{.secrets[].name}{"\n"}') -o jsonpath="{.data.token}" | base64 -D
   74  kubectl -n drone-release get secret $(kubectl -n drone-release get sa drone-release -o jsonpath='{.secrets[].name}{"\n"}') -o jsonpath="{.data.token}"
   75  gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project devops-cc
   76  kubectl get secrets
   77  kubectl config view -o jsonpath='{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
   78  gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project devops-cc
   79  echo http://$SERVICE_IP/
   80  export SERVICE_IP=$(kubectl get svc drone-release-drone -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
   81  echo http://$SERVICE_IP/
   82  kubectl get secret drone-release-drone -o jsonpath='{.data.ca\.crt}' && echo
   83  kubectl get secrets
   84  kubectl get secret drone-release-drone-pipeline-token-2sv8j -o jsonpath='{.data.ca\.crt}' && echo
   85  kubectl get secret drone-release-drone-pipeline-token-2sv8j -o jsonpath='{.data.token}' | base64 --decode && echo
   86  kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.token}' | base64 --decode && echo
   87  kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.ca\.crt}' && echo
   88  kubectl config view
   89  curl http://localhost:8080/api/
   90  curl http://localhost:52000/api/
   91  kubectl get secrets
   92  kubectl get secret default-token-bkzjn -o jsonpath='{.data.ca\.crt}' && echo
      93  kubectl get secret drone-release-drone -o jsonpath='{.data.token}' | base64 --decode && echo
   94  kubectl get secret default-token-bkzjn -o jsonpath='{.data.token}' | base64 --decode && echo
   95  kubectl config view -o jsonpath='{"Cluster name\tServer\n"}{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
   96  APISERVER=$(kubectl config view -o jsonpath="{.clusters[?(@.name==\"$CLUSTER_NAME\")].cluster.server}")
   97  echo $APISERVER
   98  kubectl config view -o jsonpath="{.clusters[?(@.name==\"gke_devops-cc_us-central1-c_cluster-1\")].cluster.server}"
   99  export CLUSTER_NAME="gke_devops-cc_us-central1-c_cluster-1"
  100  APISERVER=$(kubectl config view -o jsonpath="{.clusters[?(@.name==\"$CLUSTER_NAME\")].cluster.server}")
  101  TOKEN=$(kubectl get secrets -o jsonpath="{.items[?(@.metadata.annotations['kubernetes\.io/service-account\.name']=='default')].data.token}"|base64 --decode)
  102  curl -X GET $APISERVER/api --header "Authorization: Bearer $TOKEN" --insecure
  103  kubectl get secrets
  104  kubectl get secret drone-release-drone-pipeline-token-2sv8j -o jsonpath='{.data.token}' | base64 --decode && echo
  105  kubectl get secret drone-release-drone-pipeline-token-2sv8j -o jsonpath='{.data.ca\.crt}' && echo
  106  kubectl get svc
  107  kubectl get sa --all-namespaces | grep default
  108  kubectl get sa default -o yaml
  109  kubectl get secret default-token-bkzjn -o jsonpath='{.data.ca\.crt}' && echo
  110  kubectl get secret default-token-bkzjn -o yaml
  111  gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project devops-cc
  112  kubectl config view -o jsonpath='{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
  113  kubectl get secrets
  114  kubectl get nodes -o wide
  115  kubectl config view -o jsonpath='{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
  116  kubectl get secret gke-cluster-1-default-pool-b2acec7b-cxg3 -o jsonpath='{.data.ca\.crt}' && echo
  117  kubectl get secrets
  118  gcloud container clusters get-credentials sample-cluster
  119  gcloud container clusters get-credentials sample-cluster --zone=us-central1-c
  120  gcloud container clusters get-credentials sample-cluster --zone=us-central
  121  gcloud container clusters get-credentials sample-cluster --zone=us-central1-c
  122  gcloud container clusters get-credentials cluster-1 --zone=us-central1-c
  123  ll .kube/
  124  cat .kube/config
  125  kubectl get nodes -o wide
  126  gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project devops-cc  && kubectl get node gke-cluster-1-default-pool-b2acec7b-cxg3 -o yaml
  127  ll
  128  ll ~/.ssh/admin-cluster.key
  129  ll .kube/
  130  ll .kube/config
  131  cat .kube/config
  132  kubectl get nodes -o wide
  133  kubectl get secrets
  134  history
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get nodes -o wide
NAME                                       STATUS   ROLES    AGE   VERSION           INTERNAL-IP   EXTERNAL-IP     OS-IMAGE                             KERNEL-VERSION   CONTAINER-RUNTIME
gke-cluster-1-default-pool-b2acec7b-5jht   Ready    <none>   19h   v1.14.10-gke.27   10.128.0.5    XXXXXXX   Container-Optimized OS from Google   4.14.138+        docker://18.9.7
gke-cluster-1-default-pool-b2acec7b-cxg3   Ready    <none>   19h   v1.14.10-gke.27   10.128.0.6    XXXXXXX   Container-Optimized OS from Google   4.14.138+        docker://18.9.7
gke-cluster-1-default-pool-b2acec7b-rr85   Ready    <none>   19h   v1.14.10-gke.27   10.128.0.7    XXXXXXX   Container-Optimized OS from Google   4.14.138+        docker://18.9.7
wagnerdevel@cloudshell:~ (devops-cc)$ helm upgrade drone-release --reuse-values --set 'server.host=http://XXXXXXX:32000' stable/drone
Release "drone-release" has been upgraded.
LAST DEPLOYED: Fri Apr 17 17:07:53 2020
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ClusterRole
NAME                          AGE
drone-release-drone-pipeline  19h

==> v1/ClusterRoleBinding
NAME                          AGE
drone-release-drone-pipeline  19h

==> v1/Deployment
NAME                        READY  UP-TO-DATE  AVAILABLE  AGE
drone-release-drone-server  1/1    1           1          19h

==> v1/PersistentVolumeClaim
NAME                 STATUS  VOLUME                                    CAPACITY  ACCESS MODES  STORAGECLASS  AGE
drone-release-drone  Bound   XXXXXXX  1Gi       RWO           standard      19h

==> v1/Pod(related)
NAME                                         READY  STATUS             RESTARTS  AGE
drone-release-drone-server-5bb66d96bf-78pwv  0/1    Terminating        0         19h
drone-release-drone-server-7cd766796b-8xnxz  1/1    Running            0         19h
drone-release-drone-server-f79df9fb8-6wx4q   0/1    ContainerCreating  0         0s

==> v1/Role
NAME                 AGE
drone-release-drone  19h

==> v1/RoleBinding
NAME                 AGE
drone-release-drone  19h

==> v1/Secret
NAME                 TYPE    DATA  AGE
drone-release-drone  Opaque  1     19h

==> v1/Service
NAME                 TYPE          CLUSTER-IP  EXTERNAL-IP   PORT(S)       AGE
drone-release-drone  LoadBalancer  10.8.3.121  XXXXXXX  80:32000/TCP  19h

==> v1/ServiceAccount
NAME                          SECRETS  AGE
drone-release-drone           1        19h
drone-release-drone-pipeline  1        19h


NOTES:
##############################################################################
This chart has been deprecated in favor of the official Drone chart:
https://github.com/drone/charts
##############################################################################
*********************************************************************************
***        PLEASE BE PATIENT: drone may take a few minutes to install         ***
*********************************************************************************

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc -w drone-release-drone'

Get the Drone URL by running:
  export SERVICE_IP=$(kubectl get svc drone-release-drone -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
  echo http://$SERVICE_IP/



wagnerdevel@cloudshell:~ (devops-cc)$ export SERVICE_IP=$(kubectl get svc drone-release-drone -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
wagnerdevel@cloudshell:~ (devops-cc)$ echo http://$SERVICE_IP/
http://XXXXXXX/
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secrets
NAME                                       TYPE                                  DATA   AGE
default-token-bkzjn                        kubernetes.io/service-account-token   3      19h
drone-release-drone                        Opaque                                1      19h
drone-release-drone-pipeline-token-2sv8j   kubernetes.io/service-account-token   3      19h
drone-release-drone-token-7x8d9            kubernetes.io/service-account-token   3      19h
drone-server-secrets                       Opaque                                1      19h
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.ca\.crt}' && echo
XXXXXXX
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.ca\.crt}' && echo
XXXXXXX
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.token}' | base64 --decode && echo
XXXXXXX
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.token}' | base64 --decode && echo
XXXXXXX
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.token}' | base64 --decode && echo
XXXXXXX
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secret drone-release-drone-token-7x8d9 -o jsonpath='{.data.token}' | base64 --decode && echo
XXXXXXX
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secrets
NAME                                       TYPE                                  DATA   AGE
default-token-bkzjn                        kubernetes.io/service-account-token   3      19h
drone-release-drone                        Opaque                                1      19h
drone-release-drone-pipeline-token-2sv8j   kubernetes.io/service-account-token   3      19h
drone-release-drone-token-7x8d9            kubernetes.io/service-account-token   3      19h
drone-server-secrets                       Opaque                                1      19h
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secret drone-release-drone-pipeline-token-2sv8j -o jsonpath='{.data.ca\.crt}' && echo
XXXXXX
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get secret drone-release-drone-pipeline-token-2sv8j -o jsonpath='{.data.token}' | base64 --decode && echo
XXXXXX
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl config view -o jsonpath='{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
gke_devops-cc_us-central1-c_cluster-1   https://XXXXXXX

wagnerdevel@cloudshell:~ (devops-cc)$ kubectl get serviceaccounts
NAME                           SECRETS   AGE
default                        1         20h
drone-release-drone            1         19h
drone-release-drone-pipeline   1         19h

wagnerdevel@cloudshell:~ (devops-cc)$ kubectl describe secrets default
Name:         default-token-bkzjn
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: default
              kubernetes.io/service-account.uid: dc29c51d-8040-11ea-9137-42010a8000da

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1115 bytes
namespace:  7 bytes
token:      XXXXXXXX

wagnerdevel@cloudshell:~ (devops-cc)$ kubectl describe secrets drone-release-drone
Name:         drone-release-drone
Namespace:    default
Labels:       app=drone
              chart=drone-2.7.2
              heritage=Tiller
              release=drone-release
Annotations:  <none>

Type:  Opaque

Data
====
secret:  24 bytes
wagnerdevel@cloudshell:~ (devops-cc)$ kubectl describe secrets drone-release-drone-pipeline
Name:         drone-release-drone-pipeline-token-2sv8j
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: drone-release-drone-pipeline
              kubernetes.io/service-account.uid: 1febffe7-8046-11ea-9137-42010a8000da

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1115 bytes
namespace:  7 bytes
token:      XXXXXX
```

### Resultados obtidos

Os resultados obtidos são apresentados nas imagens abaixo.

![App e teste](https://raw.githubusercontent.com/wagnerdevel/setrem-cicd/master/screens/screen-1.png?raw=true)

![App e teste](https://raw.githubusercontent.com/wagnerdevel/setrem-cicd/master/screens/screen-2.png?raw=true)

![App e teste](https://raw.githubusercontent.com/wagnerdevel/setrem-cicd/master/screens/screen-3.png?raw=true)

![App e teste](https://raw.githubusercontent.com/wagnerdevel/setrem-cicd/master/screens/screen-4.png?raw=true)
