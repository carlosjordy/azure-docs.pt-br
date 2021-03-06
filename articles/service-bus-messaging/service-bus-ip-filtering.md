---
title: Configuração de regras de firewall IP para o Barramento de Serviço do Azure
description: Como usar Regras de Firewall para permitir conexões de endereços IP específicos para o Barramento de Serviço do Azure.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: a5ae491f82e73c5364788dff8b531e81d17ebb68
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341438"
---
# <a name="configure-ip-firewall-rules-for-azure-service-bus"></a>Configuração de regras de firewall IP para o Barramento de Serviço do Azure
Por padrão, os namespaces de Barramento de Serviço são acessíveis pela Internet, desde que a solicitação acompanhe uma autenticação e uma autorização válidas. Com o firewall de IP, você pode restringir ainda mais a um conjunto de endereços IPv4 ou intervalos de endereços IPv4 na notação [CIDR (roteamento entre domínios sem classificação)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).

Esse recurso é útil em cenários nos quais o Barramento de Serviço do Azure deve ser acessível apenas através determinados sites conhecidos. As regras de firewall permitem que você configure regras para aceitar o tráfego originado de endereços IPv4 específicos. Por exemplo, se usar o Barramento de Serviço com o [Azure ExpressRoute][express-route], crie uma **regra de firewall** para permitir o tráfego apenas de seus endereços IP da infraestrutura local ou de endereços de um gateway da NAT corporativo. 

> [!IMPORTANT]
> Os Firewalls e as Redes Virtuais têm suporte apenas na camada **premium** do Barramento de Serviço. Se a atualização para a camada **premium** não for uma opção, recomendamos que você proteja o token de Assinatura de Acesso Compartilhado (SAS) e compartilhe-o apenas com usuários autorizados. Para obter informações sobre a autenticação SAS, veja [Autenticação e autorização](service-bus-authentication-and-authorization.md#shared-access-signature).

## <a name="ip-firewall-rules"></a>Regras de firewall de IP
As regras de firewall de IP são aplicadas no nível de namespace do Barramento de Serviço. Portanto, as regras se aplicam a todas as conexões de clientes que usam qualquer protocolo com suporte. Qualquer tentativa de conexão de um endereço IP que não corresponda a uma regra IP permitida no namespace do Barramento de Serviço será rejeitada como não autorizada. A resposta não menciona a regra IP. As regras de filtro IP são aplicadas na ordem e a primeira regra que corresponde ao endereço IP determina a ação de aceitar ou rejeitar.

>[!WARNING]
> Implementar as regras de Firewall pode impedir outros serviços do Azure de interagir com o Barramento de Serviço.
>
> Não há suporte para serviços confiáveis da Microsoft para quando a Filtragem de IP (regras de Firewall) é implementada e serão disponibilizados em breve.
>
> Cenários comuns do Azure que não funcionam com a Filtragem de IP (observe que a lista **NÃO** é exaustiva):
> - Integração com a Grade de Eventos do Azure
> - Rotas do Hub IoT do Azure
> - Device Explorer do Azure IoT
>
> Os serviços da Microsoft a seguir devem estar em uma rede virtual
> - Serviço de aplicativo do Azure
> - Funções do Azure

## <a name="use-azure-portal"></a>Usar o portal do Azure
Esta seção mostra como usar o portal do Azure para criar regras de firewall de IP para um namespace do Barramento de Serviço. 

1. Navegue até o **namespace do Barramento de Serviço** no [portal do Azure](https://portal.azure.com).
2. No menu à esquerda, selecione a opção **Rede**. Por padrão, a opção **Todas as redes** está selecionada. O namespace do Barramento de Serviço aceita conexões de qualquer endereço IP. Essa configuração padrão é equivalente a uma regra que aceita o intervalo de endereços IP 0.0.0.0/0. 

    ![Firewall – opção "Todas as redes" selecionada](./media/service-bus-ip-filtering/firewall-all-networks-selected.png)
1. Selecione a opção **Redes selecionadas** na parte superior da página. Na seção **Firewall**, siga estas etapas:
    1. Selecione a opção **Adicionar o endereço IP do cliente** para permitir ao IP do cliente atual acesso ao namespace. 
    2. Para **intervalo de endereços**, insira um endereço IPv4 específico ou um intervalo de endereços IPv4 na notação CIDR. 
    3. Especifique se você deseja **permitir que serviços confiáveis da Microsoft ignorem esse firewall**. 

        > [!WARNING]
        > Se escolher a opção **Redes selecionadas** e não especificar um endereço IP ou intervalo de endereços, o serviço permitirá o tráfego de todas as redes. 

        ![Firewall – opção "Todas as redes" selecionada](./media/service-bus-ip-filtering/firewall-selected-networks-trusted-access-disabled.png)
3. Selecione **Salvar** na barra de ferramentas para salvar as configurações. Aguarde alguns minutos para que a confirmação seja exibida nas notificações do portal.

## <a name="use-resource-manager-template"></a>Usar modelo do Resource Manager
Esta seção tem um modelo do Azure Resource Manager de exemplo que cria uma rede virtual e uma regra de firewall.


O modelo do Resource Manager a seguir permite incluir uma regra da rede virtual em um namespace de Barramento de Serviço existente.

Parâmetros de modelo:

- A **ipMask** é um endereço IPv4 único ou um bloco de endereços IP na notação CIDR. Por exemplo, na notação CIDR 70.37.104.0/24, representa os 256 endereços IPv4 de 70.37.104.0 a 70.37.104.255, em que 24 indica o número de bits de prefixo significativos para o intervalo.

> [!NOTE]
> Embora não haja nenhuma regra de negação possível, o modelo do Azure Resource Manager tem a ação padrão definida como **"Allow"** , que não restringe as conexões.
> Ao criar as regras de rede virtual ou de firewalls, devemos alterar a ***"defaultAction"***
> 
> de
> ```json
> "defaultAction": "Allow"
> ```
> para
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "servicebusNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Service Bus namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('servicebusNamespaceName')]",
        "type": "Microsoft.ServiceBus/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Premium",
          "tier": "Premium"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,          
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Para implantar o modelo, siga as instruções para o [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Próximas etapas

Para restringir o acesso a Barramento de Serviço para redes virtuais do Azure, consulte o link a seguir:

- [Pontos de Extremidade de Serviço de Rede Virtual para Barramento de Serviço][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
