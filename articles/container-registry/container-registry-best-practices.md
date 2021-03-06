---
title: Práticas recomendadas do registro
description: Saiba como usar o registro de contêiner do Azure efetivamente seguindo estas melhores práticas.
ms.topic: article
ms.date: 09/27/2018
ms.openlocfilehash: fc84fb8cb98f58e28570095370d55a7358ce3a99
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682691"
---
# <a name="best-practices-for-azure-container-registry"></a>Melhores práticas para o Registro de Contêiner do Azure

Ao seguir essas melhores práticas, você pode ajudar a maximizar o desempenho e o uso econômico de seu registro de Docker privado no Azure.

Consulte também [Recomendações para marcação e controle de versão de imagens de contêiner](container-registry-image-tag-version.md) para obter estratégias para marcação e as imagens de versão no registro. 

## <a name="network-close-deployment"></a>Implantação de fechamento de rede

Crie o registro de contêiner na mesma região do Azure em que você implantar contêineres. Colocar o registro em uma região de fechamento de rede para seus hosts de contêiner pode ajudar a reduzir a latência e o custo.

A implantação de fechamento de rede é um dos principais motivos para usar um registro de contêiner particular. As imagens do docker têm uma [construção de camadas](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) eficiente, que permite implantações incrementais. No entanto, os novos nós precisam efetuar o pull de todas as camadas necessárias para uma determinada imagem. Este `docker pull` inicial pode acumular até vários gigabytes. Ter um registro privado perto de sua implantação minimiza a latência de rede.
Além disso, todas as nuvens públicas, incluindo do Azure, implementam taxas de saída de rede. Extrair imagens de um datacenter para outro adiciona taxas de saída de rede, além da latência.

## <a name="geo-replicate-multi-region-deployments"></a>Replicar geograficamente implantações em várias regiões

Use o recurso de [replicação geográfica](container-registry-geo-replication.md) do Registro de Contêiner do Azure se estiver implantando contêineres em várias regiões. Se você estiver atendendo clientes globais de centros de dados locais ou se sua equipe de desenvolvimento estiver em locais diferentes, é possível simplificar o gerenciamento de registro e minimizar a latência por meio da replicação geográfica do registro. A replicação geográfica está disponível apenas com registros [Premium](container-registry-skus.md).

Para saber como usar a replicação geográfica, consulte o tutorial em três partes, [Replicação geográfica no Registro de Contêiner do Azure](container-registry-tutorial-prepare-registry.md).

## <a name="repository-namespaces"></a>Namespaces do repositório

Ao aproveitar os namespaces de repositório, você pode permitir o compartilhamento de um único registro em vários grupos em sua organização. Os registros podem ser compartilhados entre equipes e implantações. O Registro de Contêiner do Azure oferece suporte a namespaces aninhados, habilitando o isolamento de grupo.

Por exemplo, considere as seguintes marcas de imagem de contêiner. Imagens que são usadas em toda a empresa, como `aspnetcore`, são colocadas no namespace raiz, enquanto as imagens de contêiner que pertencem aos grupos de Produtos e Marketing usam cada um seus próprios namespaces.

- *contoso.azurecr.io/aspnetcore:2.0*
- *contoso.azurecr.io/products/widget/web:1*
- *contoso.azurecr.io/products/bettermousetrap/refundapi:12.3*
- *contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42*

## <a name="dedicated-resource-group"></a>Grupo de recursos dedicados

Como os registros de contêiner são recursos que são usados em vários hosts de contêiner, um registro deve residir seu próprio grupo de recursos.

Embora seja possível fazer experiências com um tipo de host específico, como Instâncias de Contêiner do Azure, você provavelmente desejará excluir a instância de contêiner quando terminar. No entanto, você também poderá manter a coleção de imagens que enviou por push para o Registro de Contêiner do Azure. Ao colocar o registro em seu próprio grupo de recursos, você minimiza o risco de excluir acidentalmente a coleção de imagens no registro quando exclui o grupo de recursos de instância do contêiner.

## <a name="authentication"></a>Autenticação

Ao se autenticar com um registro de contêiner do Azure, há dois cenários principais: autenticação individual e autenticação de serviço (ou "sem periférico"). A tabela a seguir fornece uma visão geral sobre esses cenários e o método de autenticação recomendado para cada.

| Type | Cenário de exemplo | Método recomendado |
|---|---|---|
| Identidade individual | Um desenvolvedor que efetua o pull de imagens de ou para seu computador de desenvolvimento. | [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) |
| Sem periférico/serviço identidade | Pipelines de implantação e compilação em que o usuário não está diretamente envolvido. | [Entidade de serviço](container-registry-authentication.md#service-principal) |

Para obter informações detalhadas sobre a autenticação de Registro de Contêiner do Azure, consulte [Autenticar com um registro de contêiner do Azure](container-registry-authentication.md).

## <a name="manage-registry-size"></a>Gerenciar o tamanho do registro

As restrições de armazenamento de cada [camada de serviço de registro de contêiner][container-registry-skus] são projetados para se alinhar com um cenário típico: **Básico** para começar, **Standard** para a maioria dos aplicativos de produção e **Premium** para o desempenho de hiperdimensionamento e [replicação geográfica][container-registry-geo-replication]. Durante a vida útil do registro, você deve gerenciar o tamanho periodicamente excluindo conteúdo não utilizado.

Use o comando da CLI do Azure [az acr show-usage][az-acr-show-usage] para exibir o tamanho atual do registro:

```azurecli
az acr show-usage --resource-group myResourceGroup --name myregistry --output table
```

```output
NAME      LIMIT         CURRENT VALUE    UNIT
--------  ------------  ---------------  ------
Size      536870912000  185444288        Bytes
Webhooks  100                            Count
```

Você também pode encontrar o armazenamento atual usado na **Visão geral** do seu registro no portal do Azure:

![Informações de uso do registro no portal do Azure][registry-overview-quotas]

### <a name="delete-image-data"></a>Excluir dados de imagem

O Registro de Contêiner do Azure dá suporte a vários métodos para excluir dados de imagem do registro de contêiner. É possível excluir imagens por marca, resumo de manifesto ou excluir um repositório inteiro.

Para obter detalhes sobre como excluir dados de imagem do registro, incluindo imagens não marcadas (às vezes chamadas de "pendentes" ou "órfãs"), consulte [Excluir imagens de contêiner no Registro de Contêiner do Azure](container-registry-delete.md).

## <a name="next-steps"></a>Próximas etapas

O Registro de Contêiner do Azure está disponível em várias camadas (também chamadas de SKUs) e cada uma delas fornece recursos diferentes. Para obter detalhes sobre as camadas de serviço disponíveis, consulte [Camadas de serviço do Registro de Contêiner do Azure](container-registry-skus.md).

<!-- IMAGES -->
[delete-repository-portal]: ./media/container-registry-best-practices/delete-repository-portal.png
[registry-overview-quotas]: ./media/container-registry-best-practices/registry-overview-quotas.png

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-show-usage]: /cli/azure/acr#az-acr-show-usage
[azure-cli]: /cli/azure
[azure-portal]: https://portal.azure.com
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-skus]: container-registry-skus.md
