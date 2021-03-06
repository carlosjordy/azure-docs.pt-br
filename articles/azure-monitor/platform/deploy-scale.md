---
title: Implantar Azure Monitor
description: Implante Azure Monitor recursos em escala usando Azure Policy.
ms.subservice: ''
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: fbfc0cafe83f53bd7cab2b93899e9c2cb02d52e3
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86505203"
---
# <a name="deploy-azure-monitor-at-scale-using-azure-policy"></a>Implantar Azure Monitor em escala usando Azure Policy
Embora alguns recursos de Azure Monitor sejam configurados uma vez ou um número limitado de vezes, outros devem ser repetidos para cada recurso que você deseja monitorar. Este artigo descreve métodos para usar o Azure Policy para implementar Azure Monitor em escala para garantir que o monitoramento seja configurado de forma consistente e precisa para todos os seus recursos do Azure.

Por exemplo, você precisa criar uma configuração de diagnóstico para todos os recursos existentes do Azure e para cada novo recurso que você criar. Você também precisa ter um agente instalado e configurado sempre que criar uma máquina virtual. Você pode usar métodos como o PowerShell ou a CLI para executar essas ações, já que esses métodos estão disponíveis para todos os recursos do Azure Monitor. Usando Azure Policy, você pode ter uma lógica em vigor que executará automaticamente a configuração apropriada sempre que você criar ou modificar um recurso.


## <a name="azure-policy"></a>Azure Policy
Esta seção fornece uma breve introdução ao [Azure Policy](../../governance/policy/overview.md) que permite avaliar e impor padrões organizacionais em toda a sua assinatura do Azure ou grupo de gerenciamento com esforço mínimo. Consulte a [documentação do Azure Policy](../../governance/policy/overview.md) para obter detalhes completos.

Com Azure Policy você pode especificar requisitos de configuração para todos os recursos que são criados e identificar os recursos que estão fora de conformidade, impedir que os recursos sejam criados ou adicionar a configuração necessária. Ele funciona interceptando chamadas para criar um novo recurso ou modificar um recurso existente. Ele pode responder com esses efeitos como negar a solicitação se não corresponder à correspondência com as propriedades esperadas em uma definição de política, sinalizando-a para não conformidade ou implantando um recurso relacionado. Você pode corrigir os recursos existentes com uma definição de política **deployIfNotExists** ou **Modify** .

Azure Policy consiste nos objetos na tabela a seguir. Consulte [Azure Policy objetos](../../governance/policy/overview.md#azure-policy-objects) para obter uma explicação mais detalhada de cada um.

| Item | Descrição |
|:---|:---|
| Definição de política | Descreve as condições de conformidade do recurso e o efeito a ser tomada se uma condição for atendida. Isso pode ser todos os recursos de um tipo específico ou apenas recursos que correspondem a determinadas propriedades. O efeito pode ser simplesmente sinalizar o recurso quanto à conformidade ou implantar um recurso relacionado. As definições de política são escritas usando JSON, conforme descrito em [estrutura de definição de Azure Policy](../../governance/policy/concepts/definition-structure.md). Os efeitos são descritos em [entender Azure Policy efeitos](../../governance/policy/concepts/effects.md).
| Iniciativa de política | Um grupo de definições de política que devem ser aplicadas juntas. Por exemplo, você pode ter uma definição de política para enviar logs de recursos para um espaço de trabalho Log Analytics e outro para enviar logs de recursos para os hubs de eventos. Crie uma iniciativa que inclui as duas definições de política e aplique a iniciativa aos recursos em vez das definições de política individuais. As iniciativas são escritas usando JSON, conforme descrito em [Azure Policy estrutura de iniciativa](../../governance/policy/concepts/initiative-definition-structure.md). |
| Atribuição | Uma definição ou iniciativa de política não entrará em vigor até ser atribuída a um escopo. Por exemplo, atribua uma política a um grupo de recursos para aplicá-la a todos os recursos criados nesse recurso ou aplique-a a uma assinatura para aplicá-la a todos os recursos nessa assinatura.  Para obter mais detalhes, consulte [Azure Policy estrutura de atribuição](../../governance/policy/concepts/assignment-structure.md). |

## <a name="built-in-policy-definitions-for-azure-monitor"></a>Definições de política internas para o Azure Monitor
Azure Policy inclui várias definições predefinidas relacionadas a Azure Monitor. Você pode atribuir essas definições de política à sua assinatura existente ou usá-las como base para criar suas próprias definições personalizadas. Para obter uma lista completa do política interna na categoria **monitoramento** , consulte [Azure Policy definições de política internas para Azure monitor](../samples/policy-samples.md).

Para exibir as definições de política internas relacionadas ao monitoramento, faça o seguinte:

1. Vá para **Azure Policy** na portal do Azure.
2. Selecione **definições**.
3. Para **tipo**, selecione *interno* e para **categoria**, selecione *monitoramento*.

  ![Definições de política internas](media/deploy-scale/builtin-policies.png)


## <a name="diagnostic-settings"></a>Configurações de diagnóstico
[As configurações de diagnóstico](../platform/diagnostic-settings.md) coletam logs de recursos e métricas de recursos do Azure para vários locais, normalmente para um espaço de trabalho log Analytics que permite que você analise os dados com [consultas de log](../log-query/log-query-overview.md) e [alertas de log](alerts-log.md). Use a política para criar automaticamente uma configuração de diagnóstico cada vez que você criar um recurso.

Cada tipo de recurso do Azure tem um conjunto exclusivo de categorias que precisam ser listadas na configuração de diagnóstico. Por isso, cada tipo de recurso requer uma definição de política separada. Alguns tipos de recursos têm definições de políticas internas que você pode atribuir sem modificação. Para outros tipos de recursos, você precisa criar uma definição personalizada.

### <a name="built-in-policy-definitions-for-azure-monitor"></a>Definições de política internas para o Azure Monitor
Há duas definições de política internas para cada tipo de recurso, uma para enviar para Log Analytics espaço de trabalho e outra para o Hub de eventos. Se você precisar apenas de um local, atribua essa política para o tipo de recurso. Se você precisar de ambos, atribua as duas definições de política para o recurso.

Por exemplo, a imagem a seguir mostra as definições de política de configuração de diagnóstico interno para Data Lake Analytics.

  ![Definições de política internas](media/deploy-scale/builtin-diagnostic-settings.png)

### <a name="custom-policy-definitions"></a>Definições de política personalizadas
Para tipos de recursos que não têm uma política interna, você precisa criar uma definição de política personalizada. Você pode fazer isso manualmente no portal do Azure copiando uma política interna existente e, em seguida, modificando para o tipo de recurso. No entanto, é mais eficiente criar a política de forma programática usando um script na Galeria do PowerShell.

O script [Create-AzDiagPolicy](https://www.powershellgallery.com/packages/Create-AzDiagPolicy) cria arquivos de política para um tipo de recurso específico que você pode instalar usando o PowerShell ou a CLI. Use o procedimento a seguir para criar uma definição de política personalizada para configurações de diagnóstico.


1. Verifique se você tem [Azure PowerShell](/powershell/azure/install-az-ps) instalado.
2. Instale o script com o seguinte comando:
  
    ```azurepowershell
    Install-Script -Name Create-AzDiagPolicy
    ```

3. Execute o script usando os parâmetros para especificar para onde enviar os logs. Você será solicitado a especificar uma assinatura e um tipo de recurso. Por exemplo, para criar uma definição de política que envia para Log Analytics espaço de trabalho e Hub de eventos, use o comando a seguir.

   ```azurepowershell
   Create-AzDiagPolicy.ps1 -ExportLA -ExportEH -ExportDir ".\PolicyFiles"  
   ```

4. Como alternativa, você pode especificar uma assinatura e um tipo de recurso no comando. Por exemplo, para criar uma definição de política que envia para Log Analytics espaço de trabalho e o Hub de eventos para bancos de dados do Azure SQL Server, use o comando a seguir.

   ```azurepowershell
   Create-AzDiagPolicy.ps1 -SubscriptionID xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -ResourceType Microsoft.Sql/servers/databases  -ExportLA -ExportEH -ExportDir ".\PolicyFiles"  
   ```

5. O script cria pastas separadas para cada definição de política, cada uma contendo três arquivos chamados azurepolicy, JSON, azurepolicy.rules.json, azurepolicy.parameters.jsem. Se você quiser criar a política manualmente no portal do Azure, poderá copiar e colar o conteúdo de azurepolicy.jsno, já que ele inclui a definição de política inteira. Use os outros dois arquivos com o PowerShell ou a CLI para criar a definição de política a partir de uma linha de comando.

    Os exemplos a seguir mostram como instalar a definição de política do PowerShell e da CLI. Cada um inclui metadados para especificar uma categoria de **monitoramento** para agrupar a nova definição de política com as definições de política internas.

      ```azurepowershell
      New-AzPolicyDefinition -name "Deploy Diagnostic Settings for SQL Server database to Log Analytics workspace" -policy .\Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.rules.json -parameter .\Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.parameters.json -mode All -Metadata '{"category":"Monitoring"}'
      ```

      ```azurecli
      az policy definition create --name 'deploy-diag-setting-sql-database--workspace' --display-name 'Deploy Diagnostic Settings for SQL Server database to Log Analytics workspace'  --rules 'Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.rules.json' --params 'Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.parameters.json' --subscription 'AzureMonitor_Docs' --mode All
      ```

### <a name="initiative"></a>Iniciativa
Em vez de criar uma atribuição para cada definição de política, uma estratégia comum é criar uma iniciativa que inclua as definições de política para criar configurações de diagnóstico para cada serviço do Azure. Crie uma atribuição entre a iniciativa e um grupo de gerenciamento, assinatura ou grupo de recursos, dependendo de como você gerencia seu ambiente. Essa estratégia oferece os seguintes benefícios:

- Crie uma única atribuição para a iniciativa em vez de várias atribuições para cada tipo de recurso. Use a mesma iniciativa para vários grupos de monitoramento, assinaturas ou grupos de recursos.
- Modifique a iniciativa quando precisar adicionar um novo tipo de recurso ou destino. Por exemplo, os requisitos iniciais podem ser enviar dados somente para um espaço de trabalho Log Analytics, mas posteriormente você deseja adicionar o Hub de eventos. Modifique a iniciativa em vez de criar novas atribuições.

Consulte [criar e atribuir uma definição de iniciativa](../../governance/policy/tutorials/create-and-manage.md#create-and-assign-an-initiative-definition) para obter detalhes sobre como criar uma iniciativa. Considere as seguintes recomendações:

- Defina a **categoria** para **monitoramento** para agrupá-la com definições de política personalizadas e internas relacionadas.
- Em vez de especificar os detalhes para o espaço de trabalho Log Analytics e o Hub de eventos para definição de política incluída na iniciativa, use um parâmetro de iniciativa comum. Isso permite que você especifique facilmente um valor comum para todas as definições de política e altere esse valor, se necessário.

![Definição de iniciativa](media/deploy-scale/initiative-definition.png)

### <a name="assignment"></a>Atribuição 
Atribua a iniciativa a um grupo de gerenciamento, assinatura ou grupo de recursos do Azure, dependendo do escopo dos recursos a serem monitorados. Um [grupo de gerenciamento](../../governance/management-groups/overview.md) é particularmente útil para a diretiva de escopo, especialmente se a sua organização tiver várias assinaturas.

![Atribuição de iniciativa](media/deploy-scale/initiative-assignment.png)

Usando parâmetros de iniciativa, você pode especificar o espaço de trabalho ou qualquer outro detalhe uma vez para todas as definições de política na iniciativa. 

![Parâmetros de iniciativa](media/deploy-scale/initiative-parameters.png)

### <a name="remediation"></a>Correção
A iniciativa será aplicada a cada máquina virtual à medida que ela for criada. Uma [tarefa de correção](../../governance/policy/how-to/remediate-resources.md) implanta as definições de política na iniciativa para recursos existentes, portanto, isso permite que você crie configurações de diagnóstico para todos os recursos que já foram criados. Ao criar a atribuição usando o portal do Azure, você tem a opção de criar uma tarefa de correção ao mesmo tempo. Consulte [corrigir recursos sem conformidade com Azure Policy](../../governance/policy/how-to/remediate-resources.md) para obter detalhes sobre a correção.

![Correção da iniciativa](media/deploy-scale/initiative-remediation.png)


## <a name="azure-monitor-for-vms"></a>Azure Monitor para VMs
[Azure monitor para VMs](../insights/vminsights-overview.md) é a principal ferramenta no Azure monitor para monitorar as máquinas virtuais. Habilitar Azure Monitor para VMs instala o agente de Log Analytics e o agente de dependência. Em vez de executar essas tarefas manualmente, use Azure Policy para garantir que cada máquina virtual seja configurada ao criá-la.

O Azure Monitor para VMs inclui duas iniciativas internas chamadas **habilitar Azure monitor para VMs** e **habilitar Azure monitor para conjuntos de dimensionamento de máquinas virtuais**. Essas iniciativas incluem um conjunto de definições de política necessárias para instalar o agente de Log Analytics e o agente de dependência necessários para habilitar o Azure Monitor para VMs. 

Em vez de criar atribuições para essas iniciativas usando a interface Azure Policy, Azure Monitor para VMs inclui um recurso que permite inspecionar o número de máquinas virtuais em cada escopo para determinar se a iniciativa foi aplicada. Em seguida, você pode configurar o espaço de trabalho e criar as atribuições necessárias usando essa interface.

Para obter detalhes sobre esse processo, consulte [habilitar Azure monitor para VMs usando Azure Policy](../insights/vminsights-enable-at-scale-policy.md).

![Política de Azure Monitor para VMs](../platform/media/deploy-scale/vminsights-policy.png)


## <a name="next-steps"></a>Próximas etapas

- Leia mais sobre [Azure Policy](../../governance/policy/overview.md).
- Leia mais sobre [as configurações de diagnóstico](diagnostic-settings.md).
