---
title: Invoegtoepassing http-toepassingsroutering op Azure Kubernetes Service (AKS)
description: Gebruik de invoegtoepassing HTTP-toepassingsroutering om toegang te krijgen tot toepassingen die zijn geïmplementeerd op Azure Kubernetes Service (AKS).
services: container-service
author: lachie83
ms.topic: article
ms.date: 07/20/2020
ms.author: laevenso
ms.openlocfilehash: 95767e94a120b7f4595744beabc72fcd954e6e2f
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2021
ms.locfileid: "107783103"
---
# <a name="http-application-routing"></a>Routering van HTTP-toepassing

Met de routeringsoplossing voor HTTP-toepassingen hebt u eenvoudig toegang tot toepassingen die zijn geïmplementeerd in uw Azure Kubernetes Service (AKS)-cluster. Wanneer de oplossing is ingeschakeld, configureert deze een controller [voor ingress](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/) in uw AKS-cluster. Terwijl toepassingen worden geïmplementeerd, worden met de oplossing ook openbaar toegankelijke DNS-namen gemaakt voor toepassingseindpunten.

Wanneer de invoeg-on is ingeschakeld, wordt er een DNS-zone in uw abonnement gemaakt. Zie Prijzen voor DNS voor meer informatie [over DNS-kosten.][dns-pricing]

> [!CAUTION]
> De invoegtoepassing voor HTTP-toepassingsroutering is ontworpen om snel een controller voor toegangsgressie te maken en toegang te krijgen tot uw toepassingen. Deze invoeggebruiker is momenteel niet ontworpen voor gebruik in een productieomgeving en wordt niet aanbevolen voor productiegebruik. Zie Een [HTTPS-ingress-controller](./ingress-tls.md)maken voor implementaties die gereed zijn voor productie en die meerdere replica's en TLS-ondersteuning bevatten.

## <a name="http-routing-solution-overview"></a>Overzicht van http-routeringsoplossing

De invoegsel implementeert twee onderdelen: een [Kubernetes-controller][ingress] voor verkeer en een [extern-DNS-controller.][external-dns]

- **Controller voor toegangsgressie:** de toegangscontroller wordt blootgesteld aan internet met behulp van een Kubernetes-service van het type LoadBalancer. De toegangscontroller bewaakt en implementeert [Kubernetes-toegangsingress-resources,][ingress-resource]waarmee routes naar toepassings-eindpunten worden gemaakt.
- **Externe DNS-controller:** hiermee wordt gekeken naar kubernetes-resources voor binnenkomend verkeer en worden DNS A-records gemaakt in de clusterspecifieke DNS-zone.

## <a name="deploy-http-routing-cli"></a>HTTP-routering implementeren: CLI

De invoegtoepassing voor HTTP-toepassingsroutering kan worden ingeschakeld met de Azure CLI bij het implementeren van een AKS-cluster. Gebruik de opdracht [az aks create][az-aks-create] met het argument om dit te `--enable-addons` doen.

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --enable-addons http_application_routing
```

> [!TIP]
> Als u meerdere invoegtoepassingen wilt inschakelen, geeft u deze op als een door komma's gescheiden lijst. Als u bijvoorbeeld ROUTERING en bewaking van HTTP-toepassingen wilt inschakelen, gebruikt u de indeling `--enable-addons http_application_routing,monitoring` .

U kunt http-routering ook inschakelen op een bestaand AKS-cluster met behulp van [de opdracht az aks enable-addons.][az-aks-enable-addons] Als u HTTP-routering wilt inschakelen op een bestaand cluster, voegt u de parameter toe en http_application_routing `--addons` zoals wordt weergegeven in het volgende voorbeeld: 

```azurecli
az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing
```

Nadat het cluster is geïmplementeerd of bijgewerkt, gebruikt u de [opdracht az aks show][az-aks-show] om de naam van de DNS-zone op te halen.

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table
```

Deze naam is nodig om toepassingen te implementeren in het AKS-cluster en wordt weergegeven in de volgende voorbeelduitvoer:

```console
9f9c1fe7-21a1-416d-99cd-3543bb92e4c3.eastus.aksapp.io
```

## <a name="deploy-http-routing-portal"></a>HTTP-routering implementeren: Portal

De invoegtoepassing http-toepassingsroutering kan worden ingeschakeld via de Azure Portal bij het implementeren van een AKS-cluster.

![De functie HTTP-routering inschakelen](media/http-routing/create.png)

Nadat het cluster is geïmplementeerd, bladert u naar de automatisch gemaakte AKS-resourcegroep en selecteert u de DNS-zone. Noteer de naam van de DNS-zone. Deze naam is nodig om toepassingen te implementeren in het AKS-cluster.

![De naam van de DNS-zone op halen](media/http-routing/dns.png)

## <a name="connect-to-your-aks-cluster"></a>Verbinding maken met uw AKS-cluster

Gebruik [kubectl][kubectl], de Kubernetes-opdrachtregelclient, als u vanaf uw lokale computer verbinding wilt maken met het Kubernetes-cluster.

Als u Azure Cloud Shell gebruikt, is `kubectl` al geïnstalleerd. Als u het lokaal wilt installeren, gebruikt u de opdracht [az aks install-cli][]:

```azurecli
az aks install-cli
```

Gebruik de opdracht [az aks get-credentials][] om `kubectl` zodanig te configureren dat er verbinding wordt gemaakt met het Kubernetes-cluster. In het volgende voorbeeld worden referenties voor het AKS-cluster met de *naam MyAKSCluster* in *myResourceGroup opgeslagen:*

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKSCluster
```

## <a name="use-http-routing"></a>HTTP-routering gebruiken

De routeringsoplossing voor HTTP-toepassingen kan alleen worden geactiveerd voor ingress-resources met de volgende aantekeningen:

```yaml
annotations:
  kubernetes.io/ingress.class: addon-http-application-routing
```

Maak een bestand met de **naam samples-http-application-routing.yaml** en kopieer het in de volgende YAML. Werk op regel 43 bij met de DNS-zonenaam die in de vorige stap van dit artikel `<CLUSTER_SPECIFIC_DNS_ZONE>` is verzameld.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aks-helloworld  
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-helloworld
  template:
    metadata:
      labels:
        app: aks-helloworld
    spec:
      containers:
      - name: aks-helloworld
        image: mcr.microsoft.com/azuredocs/aks-helloworld:v1
        ports:
        - containerPort: 80
        env:
        - name: TITLE
          value: "Welcome to Azure Kubernetes Service (AKS)"
---
apiVersion: v1
kind: Service
metadata:
  name: aks-helloworld  
spec:
  type: ClusterIP
  ports:
  - port: 80
  selector:
    app: aks-helloworld
---
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: aks-helloworld
  annotations:
    kubernetes.io/ingress.class: addon-http-application-routing
spec:
  rules:
  - host: aks-helloworld.<CLUSTER_SPECIFIC_DNS_ZONE>
    http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /
```

Gebruik de [opdracht kubectl apply][kubectl-apply] om de resources te maken.

```bash
kubectl apply -f samples-http-application-routing.yaml
```

In het volgende voorbeeld ziet u de gemaakte resources:

```bash
$ kubectl apply -f samples-http-application-routing.yaml

deployment.apps/aks-helloworld created
service/aks-helloworld created
ingress.networking.k8s.io/aks-helloworld created
```

Open een webbrowser naar *aks-helloworld. \<CLUSTER_SPECIFIC_DNS_ZONE\>*, bijvoorbeeld *aks-helloworld.9f9c1fe7-21a1-416d-99cd-3543bb92e4c3.eastus.aksapp.io* en controleer of u de demotoepassing ziet. Het kan enkele minuten duren voordat de toepassing wordt weergegeven.

## <a name="remove-http-routing"></a>HTTP-routering verwijderen

De HTTP-routeringsoplossing kan worden verwijderd met behulp van de Azure CLI. Voer de volgende opdracht uit om uw AKS-cluster en resourcegroepnaam te vervangen.

```azurecli
az aks disable-addons --addons http_application_routing --name myAKSCluster --resource-group myResourceGroup --no-wait
```

Wanneer de invoegtoepassing http-toepassingsroutering is uitgeschakeld, kunnen sommige Kubernetes-resources in het cluster blijven. Deze resources omvatten *configMaps* en *geheimen* en worden gemaakt in de *naamruimte kube-system.* Als u een schoon cluster wilt onderhouden, kunt u deze resources verwijderen.

Zoek naar *resources voor addon-http-application-routing* met behulp van de volgende [kubectl][kubectl-get] get-opdrachten:

```console
kubectl get deployments --namespace kube-system
kubectl get services --namespace kube-system
kubectl get configmaps --namespace kube-system
kubectl get secrets --namespace kube-system
```

In de volgende voorbeelduitvoer ziet u configMaps die moeten worden verwijderd:

```
$ kubectl get configmaps --namespace kube-system

NAMESPACE     NAME                                                       DATA   AGE
kube-system   addon-http-application-routing-nginx-configuration         0      9m7s
kube-system   addon-http-application-routing-tcp-services                0      9m7s
kube-system   addon-http-application-routing-udp-services                0      9m7s
```

Als u resources wilt verwijderen, gebruikt u de [opdracht kubectl][kubectl-delete] delete. Geef het resourcetype, de resourcenaam en de naamruimte op. In het volgende voorbeeld wordt een van de vorige configmaps verwijderd:

```console
kubectl delete configmaps addon-http-application-routing-nginx-configuration --namespace kube-system
```

Herhaal de vorige stap voor alle resources voor routering van `kubectl delete` *invoegtoepassing-http-toepassingen* die in uw cluster zijn gebleven.

## <a name="troubleshoot"></a>Problemen oplossen

Gebruik de [opdracht kubectl logs][kubectl-logs] om de toepassingslogboeken voor de external-DNS-toepassing weer te geven. De logboeken moeten bevestigen dat een A- en TXT DNS-record zijn gemaakt.

```
$ kubectl logs -f deploy/addon-http-application-routing-external-dns -n kube-system

time="2018-04-26T20:36:19Z" level=info msg="Updating A record named 'aks-helloworld' to '52.242.28.189' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
time="2018-04-26T20:36:21Z" level=info msg="Updating TXT record named 'aks-helloworld' to '"heritage=external-dns,external-dns/owner=default"' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
```

Deze records kunnen ook worden gezien op de DNS-zoneresource in de Azure Portal.

![De DNS-records op te halen](media/http-routing/clippy.png)

Gebruik de [opdracht kubectl logs][kubectl-logs] om de toepassingslogboeken voor de Nginx Ingress-controller weer te geven. De logboeken moeten de van `CREATE` een toegangsbeheerbron en het opnieuw laden van de controller bevestigen. Alle HTTP-activiteiten worden geregistreerd.

```bash
$ kubectl logs -f deploy/addon-http-application-routing-nginx-ingress-controller -n kube-system

-------------------------------------------------------------------------------
NGINX Ingress controller
  Release:    0.13.0
  Build:      git-4bc943a
  Repository: https://github.com/kubernetes/ingress-nginx
-------------------------------------------------------------------------------

I0426 20:30:12.212936       9 flags.go:162] Watching for ingress class: addon-http-application-routing
W0426 20:30:12.213041       9 flags.go:165] only Ingress with class "addon-http-application-routing" will be processed by this ingress controller
W0426 20:30:12.213505       9 client_config.go:533] Neither --kubeconfig nor --master was specified.  Using the inClusterConfig.  This might not work.
I0426 20:30:12.213752       9 main.go:181] Creating API client for https://10.0.0.1:443
I0426 20:30:12.287928       9 main.go:225] Running in Kubernetes Cluster version v1.8 (v1.8.11) - git (clean) commit 1df6a8381669a6c753f79cb31ca2e3d57ee7c8a3 - platform linux/amd64
I0426 20:30:12.290988       9 main.go:84] validated kube-system/addon-http-application-routing-default-http-backend as the default backend
I0426 20:30:12.294314       9 main.go:105] service kube-system/addon-http-application-routing-nginx-ingress validated as source of Ingress status
I0426 20:30:12.426443       9 stat_collector.go:77] starting new nginx stats collector for Ingress controller running in namespace  (class addon-http-application-routing)
I0426 20:30:12.426509       9 stat_collector.go:78] collector extracting information from port 18080
I0426 20:30:12.448779       9 nginx.go:281] starting Ingress controller
I0426 20:30:12.463585       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-nginx-configuration", UID:"2588536c-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"559", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-nginx-configuration
I0426 20:30:12.466945       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-tcp-services", UID:"258ca065-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"561", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-tcp-services
I0426 20:30:12.467053       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-udp-services", UID:"259023bc-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"562", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-udp-services
I0426 20:30:13.649195       9 nginx.go:302] starting NGINX process...
I0426 20:30:13.649347       9 leaderelection.go:175] attempting to acquire leader lease  kube-system/ingress-controller-leader-addon-http-application-routing...
I0426 20:30:13.649776       9 controller.go:170] backend reload required
I0426 20:30:13.649800       9 stat_collector.go:34] changing prometheus collector from  to default
I0426 20:30:13.662191       9 leaderelection.go:184] successfully acquired lease kube-system/ingress-controller-leader-addon-http-application-routing
I0426 20:30:13.662292       9 status.go:196] new leader elected: addon-http-application-routing-nginx-ingress-controller-5cxntd6
I0426 20:30:13.763362       9 controller.go:179] ingress backend successfully reloaded...
I0426 21:51:55.249327       9 event.go:218] Event(v1.ObjectReference{Kind:"Ingress", Namespace:"default", Name:"aks-helloworld", UID:"092c9599-499c-11e8-a5e1-0a58ac1f0ef2", APIVersion:"extensions", ResourceVersion:"7346", FieldPath:""}): type: 'Normal' reason: 'CREATE' Ingress default/aks-helloworld
W0426 21:51:57.908771       9 controller.go:775] service default/aks-helloworld does not have any active endpoints
I0426 21:51:57.908951       9 controller.go:170] backend reload required
I0426 21:51:58.042932       9 controller.go:179] ingress backend successfully reloaded...
167.220.24.46 - [167.220.24.46] - - [26/Apr/2018:21:53:20 +0000] "GET / HTTP/1.1" 200 234 "" "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)" 197 0.001 [default-aks-helloworld-80] 10.244.0.13:8080 234 0.004 200
```

## <a name="clean-up"></a>Opschonen

Verwijder de gekoppelde Kubernetes-objecten die in dit artikel zijn gemaakt met behulp van `kubectl delete` .

```bash
kubectl delete -f samples-http-application-routing.yaml
```

In de voorbeelduitvoer ziet u dat Kubernetes-objecten zijn verwijderd.

```bash
$ kubectl delete -f samples-http-application-routing.yaml

deployment "aks-helloworld" deleted
service "aks-helloworld" deleted
ingress "aks-helloworld" deleted
```

## <a name="next-steps"></a>Volgende stappen

Zie HTTPS [Ingress on Azure Kubernetes Service (AKS) voor][ingress-https]meer informatie over het installeren van een met HTTPS beveiligde toegangscontroller in AKS.

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks#az_aks_create
[az-aks-show]: /cli/azure/aks#az_aks_show
[ingress-https]: ./ingress-tls.md
[az-aks-enable-addons]: /cli/azure/aks#az_aks_enable_addons
[az aks install-cli]: /cli/azure/aks#az_aks_install_cli
[az aks get-credentials]: /cli/azure/aks#az_aks_get_credentials

<!-- LINKS - external -->
[dns-pricing]: https://azure.microsoft.com/pricing/details/dns/
[external-dns]: https://github.com/kubernetes-incubator/external-dns
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[ingress]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[ingress-resource]: https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource
