---
title: Habilitar o suporte a ultra Disk no serviço kubernetes do Azure (AKS)
description: Saiba como habilitar e configurar o ultra disks em um cluster do AKS (serviço kubernetes do Azure)
services: container-service
ms.topic: article
ms.date: 07/10/2020
ms.openlocfilehash: 46be67a415f67e260262e5b80e5a1dad534aea79
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86530938"
---
# <a name="use-azure-ultra-disks-on-azure-kubernetes-service-preview"></a>Usar ultra discos do Azure no serviço kubernetes do Azure (versão prévia)

Os [ultra discos do Azure](../virtual-machines/linux/disks-enable-ultra-ssd.md) oferecem alta taxa de transferência, IOPS alta e armazenamento de disco consistente de baixa latência para seus aplicativos com estado. Um grande benefício dos ultra discos é a capacidade de alterar dinamicamente o desempenho do SSD junto com suas cargas de trabalho sem a necessidade de reiniciar os nós do agente. Ultra discos são adequados para cargas de trabalho com uso intensivo de dados.

## <a name="before-you-begin"></a>Antes de começar

Esse recurso só pode ser definido na criação do cluster ou no momento da criação do pool de nós.

> [!IMPORTANT]
> Os ultra discos do Azure exigem o nodepools implantado em zonas de disponibilidade e regiões que dão suporte a esses discos, bem como apenas a séries específicas da VM. Confira o [**escopo e as limitações do GA dos ultra discos**](../virtual-machines/linux/disks-enable-ultra-ssd.md#ga-scope-and-limitations).

### <a name="prerequisites"></a>Pré-requisitos

- Verifique se o `EnableUltraSSD` sinalizador de recurso está habilitado.
- Verifique se você tem a última `aks-preview` [extensão da CLI][az-extension-add] instalada.

### <a name="register-the-enableultrassd-preview-feature"></a>Registrar o `EnableUltraSSD` recurso de visualização

Para criar um cluster AKS ou um pool de nós que possa aproveitar os ultra discos, você deve habilitar o `EnableUltraSSD` sinalizador de recurso em sua assinatura.

Registre o `EnableUltraSSD` sinalizador de recurso usando o comando [AZ Feature Register][az-feature-register] , conforme mostrado no exemplo a seguir:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "EnableUltraSSD"
```

Demora alguns minutos para o status exibir *Registrado*. Você pode verificar o status de registro usando o comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableUltraSSD')].{Name:name,State:properties.state}"
```

Quando estiver pronto, atualize o registro do provedor de recursos *Microsoft.ContainerService* usando o comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

> [!IMPORTANT]
> Os recursos de visualização do AKS são consentimento de autoatendimento. As visualizações são fornecidas "no estado em que se encontram" e "como disponíveis" e são excluídas dos contratos de nível de serviço e da garantia limitada. As visualizações do AKS são parcialmente cobertas pelo suporte ao cliente com base no melhor esforço. Dessa forma, esses recursos não são destinados ao uso em produção. Para obter informações adicionais, consulte os seguintes artigos de suporte:
>
> - [Políticas de suporte do AKS](support-policies.md)
> - [Perguntas frequentes sobre o suporte do Azure.](faq.md)

### <a name="install-aks-preview-cli-extension"></a>Instalar a extensão da CLI aks-preview

Para criar um cluster AKS ou um pool de nós que possa usar ultra disks, você precisa da extensão mais recente da CLI do *AKs-Preview* . Instale a extensão de CLI do Azure *de AKs-Preview* usando o comando [AZ Extension Add][az-extension-add] ou instale todas as atualizações disponíveis usando o comando [AZ Extension Update][az-extension-update] :

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
``` 

### <a name="limitations"></a>Limitações
- Veja o [ **escopo e as limitações do GA de ultra discos**](../virtual-machines/linux/disks-enable-ultra-ssd.md#ga-scope-and-limitations)
- O intervalo de tamanho com suporte para um ultra disks está entre 100 e 1500

## <a name="create-a-new-cluster-that-can-use-ultra-disks"></a>Criar um novo cluster que pode usar ultra disks

Crie um cluster AKS capaz de aproveitar os ultra discos usando os comandos da CLI a seguir. Use o `--aks-custom-headers` sinalizador para definir o `EnableUltraSSD` recurso.

Criar um grupo de recursos do Azure:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Crie o cluster AKS com a integração do Azure AD gerenciada e o RBAC do Azure para autorização do kubernetes.

```azurecli-interactive
# Create an AKS-managed Azure AD cluster
az aks create -g MyResourceGroup -n MyManagedCluster -l westus2 --node-vm-size Standard_L8s_v2 --zones 1 2 --node-count 2 --aks-custom-headers EnableUltraSSD=true
```

Se você quiser criar clusters sem suporte a ultra Disk, poderá fazer isso omitindo o `--aks-custom-headers` parâmetro personalizado.

## <a name="enable-ultra-disks-on-an-existing-cluster"></a>Habilitar ultra discos em um cluster existente

Você pode habilitar ultra discos em clusters existentes adicionando um novo pool de nós ao cluster que dá suporte a ultra discos. Configure um novo pool de nós para usar a criptografia baseada em host usando o `--aks-custom-headers` sinalizador.


```azurecli
az aks nodepool add --name hostencrypt --cluster-name myAKSCluster --resource-group myResourceGroup --node-vm-size Standard_L8s_v2 --zones 1 2 --node-count 2 --aks-custom-headers EnableEncryptionAtHost=true
```

Se você quiser criar novos pools de nós sem suporte para ultra disks, poderá fazer isso omitindo o `--aks-custom-headers` parâmetro personalizado.

## <a name="use-ultra-disks-dynamically-with-a-storage-class"></a>Usar ultra discos dinamicamente com uma classe de armazenamento

Para usar ultra discos em nossas implantações ou conjuntos com estado, você pode usar uma [classe de armazenamento para provisionamento dinâmico](azure-disks-dynamic-pv.md).

### <a name="create-the-storage-class"></a>Criar a classe de armazenamento

Uma classe de armazenamento é usada para definir como uma unidade de armazenamento é criada dinamicamente com um volume persistente. Para obter mais informações sobre classes de armazenamento Kubernetes, consulte [Classes de Armazenamento Kubernetes][kubernetes-storage-classes].

Nesse caso, criaremos uma classe de armazenamento que faz referência a ultra disks. Crie um arquivo chamado `azure-ultra-disk-sc.yaml` e copie-o para o manifesto a seguir.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: ultra-disk-sc
provisioner: kubernetes.io/azure-disk
volumeBindingMode: WaitForFirstConsumer # optional, but recommended if you want to wait until the pod that will use this disk is created 
parameters:
  skuname: UltraSSD_LRS
  kind: managed
  cachingmode: None
  diskIopsReadWrite: "2000"  # minimum value: 2 IOPS/GiB 
  diskMbpsReadWrite: "320"   # minimum value: 0.032/GiB
```

Crie a classe de armazenamento com o comando [kubectl Apply][kubectl-apply] e especifique seu arquivo *Azure-ultra-Disk-SC. YAML* :

```console
$ kubectl apply -f azure-ultra-disk-sc.yaml


storageclass.storage.k8s.io/ultra-disk-sc created
```

## <a name="create-a-persistent-volume-claim"></a>Criar uma declaração de volume persistente

Um PVC (declaração de volume persistente) é usado para provisionar automaticamente o armazenamento com base em uma classe de armazenamento. Nesse caso, um PVC pode usar uma das classes de armazenamento pré-criadas para criar um disco gerenciado standard ou premium do Azure.

Crie um arquivo chamado `azure-ultra-disk-pvc.yaml` e copie-o para o manifesto a seguir. A declaração solicita um disco chamado `ultra-disk` que tenha *1000 GB* de tamanho com acesso *ReadWriteOnce* . A classe de armazenamento *ultra-Disk-SC* é especificada como a classe de armazenamento.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ultra-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: ultra-disk-sc
  resources:
    requests:
      storage: 1000Gi
```

Crie a declaração de volume persistente com o comando [kubectl Apply][kubectl-apply] e especifique o arquivo *Azure-ultra-Disk-PVC. YAML* :

```console
$ kubectl apply -f azure-ultra-disk-pvc.yaml

persistentvolumeclaim/ultra-disk created
```

## <a name="use-the-persistent-volume"></a>Usar o volume persistente

Quando a declaração do volume persistente tiver sido criada e o disco tiver sido provisionado com êxito, um pod poderá ser criado com acesso ao disco. O manifesto a seguir cria um pod NGINX básico que usa a declaração de volume persistente chamada *ultra-Disk* para montar o disco do Azure no caminho `/mnt/azure` .

Crie um arquivo chamado `nginx-ultra.yaml` e copie-o para o manifesto a seguir.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx-ultra
spec:
  containers:
  - name: nginx-ultra
    image: nginx
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: ultra-disk
```

Crie o pod com o comando [kubectl apply][kubectl-apply], conforme mostrado no exemplo a seguir:

```console
$ kubectl apply -f nginx-ultra.yaml

pod/nginx-ultra created
```

Agora você tem um pod em execução com o disco do Azure montado no diretório `/mnt/azure`. Essa configuração pode ser vista ao inspecionar o pod via `kubectl describe pod nginx-ultra`, conforme mostrado no exemplo condensado a seguir:

```console
$ kubectl describe pod nginx-ultra

[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azure-managed-disk
    ReadOnly:   false
  default-token-smm2n:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-smm2n
    Optional:    false
[...]
Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              2m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  2m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-smm2n"
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "pvc-faf0f176-8b8d-11e8-923b-deb28c58d242"
[...]
```


## <a name="next-steps"></a>Próximas etapas

- Para obter mais informações sobre ultra disks, consulte [usando os ultra discos do Azure](../virtual-machines/linux/disks-enable-ultra-ssd.md).
- Para obter mais informações sobre as práticas recomendadas de armazenamento, consulte [práticas recomendadas para armazenamento e backups no serviço de kubernetes do Azure (AKs)][operator-best-practices-storage]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/disks-types.md
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-snapshot-create]: /cli/azure/snapshot#az-snapshot-create
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-disk-show]: /cli/azure/disk#az-disk-show
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
[storage-class-concepts]: concepts-storage.md#storage-classes
[az-extension-add]: /cli/azure/extension?view=azure-cli-latest#az-extension-add
[az-extension-update]: /cli/azure/extension?view=azure-cli-latest#az-extension-update
[az-feature-register]: /cli/azure/feature?view=azure-cli-latest#az-feature-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
