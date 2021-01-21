---
title: Bereitstellen von BDC in einem privaten Azure Kubernetes Service-Cluster (AKS-Cluster)
titleSuffix: SQL Server Big Data Cluster
description: Erfahren Sie, wie Sie einen Big Data-Cluster in SQL Server in einem privaten Azure Kubernetes Service-Cluster (AKS-Cluster) mit erweiterten Netzwerkeinstellungen (CNI) bereitstellen.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f83c3d1e1a5bf0c9b74d058f144c4d07025c8c05
ms.sourcegitcommit: fc24f7ecc155d97e789676fffe55e45840fcb088
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620269"
---
# <a name="deploy-bdc-in-azure-kubernetes-service-aks-private-cluster"></a>Bereitstellen von BDC in einem privaten Azure Kubernetes Service-Cluster (AKS-Cluster)

In diesem Artikel wird erläutert, wie Big Data-Cluster in SQL Server in einem privaten Azure Kubernetes Service-Cluster (AKS-Cluster) bereitgestellt werden. Diese Konfiguration unterstützt die eingeschränkte Verwendung von öffentlichen IP-Adressen in einer Unternehmensnetzwerkumgebung.

Eine private Bereitstellung bietet die folgenden Vorteile:

* Eingeschränkte Verwendung öffentlicher IP-Adressen
* Der Netzwerkdatenverkehr zwischen Anwendungsservern und Clusterknotenpools erfolgt nur im privaten Netzwerk.
* Benutzerdefinierte Konfiguration für den obligatorischen ausgehenden Datenverkehr zur Erfüllung bestimmter Anforderungen

In diesem Artikel wird erläutert, wie ein privater AKS-Cluster verwendet wird, um die Verwendung öffentlicher IP-Adressen bei gleichzeitiger Anwendung entsprechender Sicherheitszeichenfolgen einzuschränken.

## <a name="deploy-private-bdc-cluster-with-aks-private-cluster"></a>Bereitstellen eines privaten BDC-Clusters mit einem privaten AKS-Cluster

Erstellen Sie zunächst einen [privaten AKS-Cluster](/azure/aks/private-clusters), um sicherzustellen, dass der Netzwerkdatenverkehr zwischen dem API-Server und den Knotenpools ausschließlich im privaten Netzwerk erfolgt. Die Steuerungsebene oder der API-Server verfügt über interne IP-Adressen in einem privaten AKS-Cluster.

In diesem Abschnitt wird beschrieben, wie Sie einen BDC-Cluster in einem privaten Azure Kubernetes Service-Cluster (AKS-Cluster) mit erweiterten Netzwerkeinstellungen (CNI) bereitstellen.

## <a name="create-a-private-aks-cluster-with-advanced-networking"></a>Erstellen eines privaten AKS-Clusters mit erweiterten Netzwerkeinstellungen

```bash

export REGION_NAME=<your Azure region >
export RESOURCE_GROUP=< your resource group name >
export SUBNET_NAME=aks-subnet
export VNET_NAME=bdc-vnet
export AKS_NAME=< your aks private cluster name >
 
az group create -n $RESOURCE_GROUP -l $REGION_NAME
 
az network vnet create \
    --resource-group $RESOURCE_GROUP \
    --location $REGION_NAME \
    --name $VNET_NAME \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name $SUBNET_NAME \
    --subnet-prefix 10.1.0.0/16
 

SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
 
echo $SUBNET_ID
## will be displayed something similar as the following: 
/subscriptions/xxxx-xxxx-xxx-xxxx-xxxxxxxx/resourceGroups/your-bdc-rg/providers/Microsoft.Network/virtualNetworks/your-aks-vnet/subnets/your-aks-subnet
```

### <a name="create-aks-private-cluster-with-advanced-networking-cni"></a>Erstellen eines privaten AKS-Clusters mit erweiterten Netzwerkeinstellungen (CNI)

Um zum nächsten Schritt zu gelangen, müssen Sie einen AKS-Cluster mit Load Balancer Standard bereitstellen, für den die Funktion für private Cluster aktiviert ist. Der von Ihnen auszuführende Befehl sieht wie folgt aus: 

```bash
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

Nach einer erfolgreichen Bereitstellung können Sie zur Ressourcengruppe `<MC_yourakscluster>` wechseln. Sie werden feststellen, dass `kube-apiserver` ein privater Endpunkt ist. Der folgende Abschnitt dient als Beispiel.

## <a name="connect-to-an-aks-cluster"></a>Herstellen einer Verbindung mit einem AKS-Cluster

```azurecli
az aks get-credentials -n $AKS_NAME -g $RESOURCE_GROUP
```

## <a name="build-big-data-cluster-bdc-deployment-profile"></a>Erstellen eines Bereitstellungsprofils für den Big Data-Cluster (BDC)

Nachdem Sie eine Verbindung mit einem AKS-Cluster hergestellt haben, können Sie mit der BDC-Bereitstellung beginnen, und Sie können die Umgebungsvariable vorbereiten und eine Bereitstellung initiieren: 

```azurecli
azdata bdc config init --source aks-dev-test --target private-bdc-aks --force
```

Generieren und Konfigurieren eines benutzerdefinierten BDC-Bereitstellungsprofils:

```azurecli
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.docker.imageTag=2019-CU6-ubuntu-16.04"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.logs.className=default"

azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"

azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="deploy-private-sql-server-big-data-cluster-with-ha"></a>Bereitstellen eines privaten Big Data-Clusters in SQL Server mit Hochverfügbarkeit

Wenn Sie einen [Big Data-Cluster in SQL Server (SQL-BDC) mit Hochverfügbarkeit bereitstellen](deployment-high-availability.md), verwenden Sie das Bereitstellungsprofil `aks-dev-test-ha`. Nach einer erfolgreichen Bereitstellung können Sie denselben `kubectl get svc`-Befehl verwenden. Es wird ein zusätzlicher `master-secondary-svc`-Dienst erstellt. Sie müssen `ServiceType` als `NodePort` konfigurieren. Andere Schritte ähneln den im vorigen Abschnitt erwähnten Schritten.

Im folgenden Beispiel wird `ServiceType` auf `NodePort` festgelegt:

```azurecli
azdata bdc config replace -c private-bdc-aks /bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
```

## <a name="deploy-bdc-in-aks-private-cluster"></a>Bereitstellen von BDC in einem privaten AKS-Cluster

```azurecli
export AZDATA_USERNAME=<your bdcadmin username>
export AZDATA_PASSWORD=< your bdcadmin password>

azdata bdc create --config-profile private-bdc-aks --accept-eula yes
```

## <a name="check-deployment-status"></a>Überprüfen des Bereitstellungsstatus

Die Bereitstellung dauert einige Minuten. Sie können den Bereitstellungsstatus mithilfe des folgenden Befehls überprüfen: 

```console
kubectl get pods -n mssql-cluster -w
```

## <a name="check-the-service-status"></a>Überprüfen des Dienststatus

Verwenden Sie den folgenden Befehl, um die Dienste zu überprüfen. Vergewissern Sie sich, dass alle Dienste fehlerfrei ausgeführt werden (ohne externe IP-Adressen):

```console
kubectl get services -n mssql-cluster
```

Lesen Sie [Verwalten von BDC in einem privaten AKS-Cluster](private-manage.md). Der nächste Schritt ist dann das [Herstellen einer Verbindung mit dem BDC-Cluster](connect-to-big-data-cluster.md).

Die Automatisierungsskripts für dieses Szenario finden Sie im [Repository für SQL Server-Beispiele auf GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/private-aks).

## <a name="next-steps"></a>Nächste Schritte

[Verwalten eines privaten Clusters](private-manage.md)

[Einschränken des ausgehenden Datenverkehrs eines privaten BDC-Clusters](private-restrict-egress-traffic.md)

[Herstellen einer Verbindung mit einem Big-Data-Cluster für SQL Server mit Azure Data Studio](connect-to-big-data-cluster.md)
