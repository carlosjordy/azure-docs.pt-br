---
title: Visão geral do agente do Connected Machine do Windows
description: Este artigo fornece uma visão geral detalhada dos agentes do Azure Arc para servidores disponíveis que oferecem suporte ao monitoramento de máquinas virtuais hospedadas em ambientes híbridos.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 07/09/2020
ms.topic: conceptual
ms.openlocfilehash: ed95b902c2c0768f50a0c6dadbfc617292932c2b
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86242943"
---
# <a name="overview-of-azure-arc-for-servers-agent"></a>Visão geral do agente do Azure Arc para servidores

O agente do Connected Machine do Azure Arc para servidores permite que você gerencie computadores Windows e Linux hospedados fora do Azure em sua rede corporativa ou em outro provedor de nuvem. Este artigo fornece uma visão geral detalhada do agente, dos requisitos do sistema e da rede e dos diferentes métodos de implantação.

## <a name="agent-component-details"></a>Detalhes do componente do agente

O pacote do agente do computador conectado do Azure contém vários componentes lógicos que são agrupados.

* O serviço de metadados de instância híbrida (HIMDS) gerencia a conexão com o Azure e a identidade do Azure do computador conectado.

* O agente de configuração de convidado fornece a funcionalidade de configuração de convidado e política de convidado, como avaliar se o computador está em conformidade com as políticas necessárias.

    Observe o seguinte comportamento com Azure Policy [configuração de convidado](../../governance/policy/concepts/guest-configuration.md) para um computador desconectado:

    * Uma atribuição de política de configuração de convidado que se destina a computadores desconectados não é afetada.
    * A atribuição de convidado é armazenada localmente por 14 dias. Dentro do período de 14 dias, se o agente da máquina conectada se reconectar ao serviço, as atribuições de política serão reaplicadas.
    * As atribuições são excluídas após 14 dias e não são reatribuídas à máquina após o período de 14 dias.

* O agente de extensão gerencia extensões de VM, incluindo instalar, desinstalar e atualizar. As extensões são baixadas do Azure e copiadas para a `%SystemDrive%\AzureConnectedMachineAgent\ExtensionService\downloads` pasta no Windows e para o Linux para o `/opt/GC_Ext/downloads` . No Windows, a extensão é instalada no caminho a seguir `%SystemDrive%\Packages\Plugins\<extension>` e, no Linux, a extensão é instalada no `/var/lib/waagent/<extension>` .

## <a name="download-agents"></a>Baixar agentes

Você pode baixar o pacote do agente do Azure Connected Machine para Windows e Linux dos locais listados abaixo.

* [Pacote do Windows Installer do agente do Windows](https://aka.ms/AzureConnectedMachineAgent) do Centro de Download da Microsoft.

* O pacote do agente do Linux é distribuído do [repositório de pacotes](https://packages.microsoft.com/) da Microsoft usando o formato de pacote preferencial para a distribuição (.RPM ou .DEB).

>[!NOTE]
>Durante essa versão prévia, apenas um pacote foi lançado, adequado para Ubuntu 16.04 ou 18.04.

O agente do Azure Connected Machine para Windows e Linux pode ser atualizado para a versão mais recente manual ou automaticamente dependendo de suas necessidades. Para saber mais, clique [aqui](manage-agent.md).

## <a name="windows-agent-installation-details"></a>Detalhes de instalação do agente Windows

O agente do Connected Machine para Windows pode ser instalado usando um destes três métodos:

* Clique duas vezes no arquivo `AzureConnectedMachineAgent.msi`.
* Manualmente, ao executar o pacote do Windows Installer `AzureConnectedMachineAgent.msi` do Shell de comando.
* De uma sessão do PowerShell usando um método com script.

Após a instalação do agente do Connected Machine para Windows, serão aplicadas as seguintes alterações de configuração adicionais em todo o sistema.

* As pastas de instalação a seguir são criadas durante a configuração.

    |Pasta |Descrição |
    |-------|------------|
    |%ProgramFiles%\AzureConnectedMachineAgent |Caminho de instalação padrão contendo os arquivos de suporte do agente.|
    |%ProgramData%\AzureConnectedMachineAgent |Contém os arquivos de configuração do agente.|
    |%ProgramData%\AzureConnectedMachineAgent\Tokens |Contém os tokens adquiridos.|
    |%ProgramData%\AzureConnectedMachineAgent\Config |Contém o arquivo de configuração do agente `agentconfig.json` com as suas informações de registro do serviço.|
    |%Systemdrive%\Arquivos de Files\ArcConnectedMachineAgent\ExtensionService\GC | Caminho de instalação que contém os arquivos do agente de configuração do convidado. |
    |%ProgramData%\GuestConfig |Contém as políticas (aplicadas) do Azure.|
    |%SystemDrive%\AzureConnectedMachineAgent\ExtensionService\downloads | As extensões são baixadas do Azure e copiadas aqui.|

* Os seguintes serviços do Windows são criados no computador de destino durante a instalação do agente.

    |Nome do serviço |Nome de exibição |Nome do processo |Descrição |
    |-------------|-------------|-------------|------------|
    |himds |Serviço de Metadados de Instância do Azure Híbrido |himds.exe |Esse serviço implementa o serviço de metadados de instância do Azure (IMDS) para gerenciar a conexão com o Azure e a identidade do Azure do computador conectado.|
    |DscService |Serviço de Configuração de Convidado |dsc_service.exe |Essa é a base de código do Desired State Configuration (DSC v2) usada no Azure a fim de implementar a Política no Convidado.|

* As variáveis ambientais a seguir são criadas durante a instalação do agente.

    |Nome |Valor padrão |Descrição |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Há vários arquivos de log disponíveis para solução de problemas. Eles são descritos na tabela a seguir.

    |Log |Descrição |
    |----|------------|
    |%ProgramData%\AzureConnectedMachineAgent\Log\himds.log |Registra os detalhes do serviço HIMDS (agentes) e a interação com o Azure.|
    |%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log |Contém a saída dos comandos da ferramenta azcmagent, quando o argumento verbose (-v) é usado.|
    |%ProgramData%\GuestConfig\gc_agent_logs\gc_agent.log |Registra detalhes da atividade de serviço de DSC,<br> em particular, a conectividade entre o serviço HIMDS e o Azure Policy.|
    |%ProgramData%\GuestConfig\gc_agent_logs\gc_agent_telemetry.txt |Registra os detalhes sobre o log detalhado e a telemetria do serviço de DSC.|
    |Ext_mgr_logs%SystemDrive%\ProgramData\GuestConfig\|Registra os detalhes sobre o componente do agente de extensão.|
    |Extension_logs%SystemDrive%\ProgramData\GuestConfig\\<Extension>|Registra os detalhes da extensão instalada.|

* O grupo de segurança local **Aplicativos de extensão de agente híbridos** é criado.

* Durante a desinstalação do agente, os artefatos a seguir não são removidos.

    * %ProgramFiles%\AzureConnectedMachineAgent\Logs
    * %ProgramData%\AzureConnectedMachineAgent e subdiretórios
    * %ProgramData%\GuestConfig

## <a name="linux-agent-installation-details"></a>Detalhes de instalação do agente Linux

O agente do Connected Machine para Linux é fornecido no formato de pacote preferencial para a distribuição (.RPM ou .DEB) hospedada no [repositório de pacotes](https://packages.microsoft.com/) da Microsoft. O agente é instalado e configurado com o pacote de script do Shell [Install_linux_azcmagent.sh](https://aka.ms/azcmagent).

Após a instalação do agente do Connected Machine para Linux, serão aplicadas as seguintes alterações de configuração adicionais em todo o sistema.

* As pastas de instalação a seguir são criadas durante a configuração.

    |Pasta |Descrição |
    |-------|------------|
    |/var/opt/azcmagent/ |Caminho de instalação padrão contendo os arquivos de suporte do agente.|
    |/opt/azcmagent/ |
    |GC_Ext/opt/ | Caminho de instalação que contém os arquivos do agente de configuração do convidado.|
    |/opt/DSC/ |
    |/var/opt/azcmagent/tokens |Contém os tokens adquiridos.|
    |/var/lib/GuestConfig |Contém as políticas (aplicadas) do Azure.|
    |/opt/GC_Ext/downloads|As extensões são baixadas do Azure e copiadas aqui.|

* Os daemons a seguir são criados no computador de destino durante a instalação do agente.

    |Nome do serviço |Nome de exibição |Nome do processo |Descrição |
    |-------------|-------------|-------------|------------|
    |himdsd. Service |Serviço de Metadados de Instância do Azure Híbrido |/opt/azcmagent/bin/himds |Esse serviço implementa o serviço de metadados de instância do Azure (IMDS) para gerenciar a conexão com o Azure e a identidade do Azure do computador conectado.|
    |dscd.service |Serviço de Configuração de Convidado |/opt/DSC/dsc_linux_service |Essa é a base de código do Desired State Configuration (DSC v2) usada no Azure a fim de implementar a Política no Convidado.|

* Há vários arquivos de log disponíveis para solução de problemas. Eles são descritos na tabela a seguir.

    |Log |Descrição |
    |----|------------|
    |/var/opt/azcmagent/log/himds.log |Registra os detalhes do serviço HIMDS (agentes) e a interação com o Azure.|
    |/var/opt/azcmagent/log/azcmagent.log |Contém a saída dos comandos da ferramenta azcmagent, quando o argumento verbose (-v) é usado.|
    |/opt/logs/dsc.log |Registra detalhes da atividade de serviço de DSC,<br> em particular, a conectividade entre o serviço do himds e o Azure Policy.|
    |/opt/logs/dsc.telemetry.txt |Registra os detalhes sobre o log detalhado e a telemetria do serviço de DSC.|
    |ext_mgr_logs/var/lib/GuestConfig/ |Registra os detalhes sobre o componente do agente de extensão.|
    |extension_logs/var/log/GuestConfig/|Registra os detalhes da extensão instalada.|

* As variáveis ambientais a seguir são criadas durante a instalação do agente. Estas variáveis são definidas em `/lib/systemd/system.conf.d/azcmagent.conf`.

    |Nome |Valor padrão |Descrição |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Durante a desinstalação do agente, os artefatos a seguir não são removidos.

    * /var/opt/azcmagent
    * /opt/logs

## <a name="prerequisites"></a>Pré-requisitos

### <a name="supported-operating-systems"></a>Sistemas operacionais compatíveis

Há suporte oficial para as seguintes versões do sistema operacional Windows e Linux para o agente do Azure Connected Machine: 

- Windows Server 2012 R2 e versões posteriores (incluindo o Windows Server Core)
- Ubuntu 16, 4 e 18, 4 (x64)
- CentOS Linux 7 (x64)
- SUSE Linux Enterprise Server (SLES) 15 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64)
- Amazon Linux 2 (x64)

>[!NOTE]
>Essa versão prévia do agente do Connected Machine para Windows só é compatível com o Windows Server configurado para usar o idioma inglês.
>

### <a name="required-permissions"></a>Permissões necessárias

* Para os computadores de integração, você é membro da função **Integração do Azure Connected Machine**.

* Para ler, modificar, reintegrar e excluir um computador, é necessário ser membro da função **Administrador de Recursos do Azure Connected Machine**. 

### <a name="azure-subscription-and-service-limits"></a>Limites de serviço e assinatura do Azure

Antes de configurar seus computadores com o Azure Arc para servidores (versão prévia), você deve examinar os [limites de assinatura](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) e os [limites do grupo de recursos](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) do Azure Resource Manager para planejar o número de computadores a ser conectado.

## <a name="tls-12-protocol"></a>Protocolo TLS 1.2

Para garantir a segurança de dados em trânsito para o Azure, incentivamos você a configurar o computador para usar o protocolo TLS 1.2. Constatou-se que versões mais antigas do protocolo TLS/protocolo SSL eram vulneráveis e embora elas ainda funcionem no momento para permitir a compatibilidade com versões anteriores, elas **não são recomendadas**.

|Plataforma/linguagem | Suporte | Mais informações |
| --- | --- | --- |
|Linux | Distribuições do Linux tendem a depender do [OpenSSL](https://www.openssl.org) para suporte a TLS 1.2. | Verifique as [OpenSSL Changelog](https://www.openssl.org/news/changelog.html) para confirmar a sua versão do OpenSSL é suportada.|
| Windows Server 2012 R2 e versões posteriores | Suporte e habilitado por padrão. | Para confirmar que você ainda está usando o [as configurações padrão](/windows-server/security/tls/tls-registry-settings).|

### <a name="networking-configuration"></a>Configuração de rede

O agente do Connected Machine para Linux e Windows comunica a saída com segurança ao Azure Arc pela porta TCP 443. Se o computador se conectar por meio de um firewall ou servidor proxy para se comunicar pela Internet, examine os requisitos abaixo para entender os requisitos da configuração de rede.

Se a conectividade de saída estiver restrita por seu firewall ou servidor proxy, verifique se as URLs listadas abaixo não estão bloqueadas. Se você permitir apenas os intervalos de IP ou nomes de domínio necessários para o agente se comunicar com o serviço, deverá permitir também o acesso às Marcas de Serviço e URLs a seguir.

Marcas de serviço:

* AzureActiveDirectory
* AzureTrafficManager

URLs:

| recurso de agente | Descrição |
|---------|---------|
|`management.azure.com`|Azure Resource Manager|
|`login.windows.net`|Active Directory do Azure|
|`dc.services.visualstudio.com`|Application Insights|
|`agentserviceapi.azure-automation.net`|Configuração de convidado|
|`*-agentservice-prod-1.azure-automation.net`|Configuração de convidado|
|`*.guestconfiguration.azure.com` |Configuração de convidado|
|`*.his.arc.azure.com`|Serviço de identidade híbrida|

Para obter uma lista de endereços IP para cada tag de serviço/região, confira o arquivo JSON – [Intervalos de IP do Azure e marcas de serviço – nuvem pública](https://www.microsoft.com/download/details.aspx?id=56519). A Microsoft publica atualizações semanais que contêm cada serviço do Azure e os intervalos de IP que ele usa. Para obter mais informações, confira [Marcas de serviço](../../virtual-network/security-overview.md#service-tags).

As URLs na tabela anterior são necessárias, além das informações sobre o intervalo de endereços IP da Marca de Serviço, porque a maioria dos serviços não tem um registro de Marca de Serviço no momento. Como tal, os endereços IP estão sujeitos a alterações. Se os intervalos de endereço IP forem necessários para sua configuração de firewall, a Marca de Serviço **AzureCloud** deverá ser usada para permitir o acesso a todos os serviços do Azure. Não desabilite o monitoramento de segurança ou a inspeção dessas URLs. Permita-os como você faria com outro tráfego de Internet.

### <a name="register-azure-resource-providers"></a>Registrar provedores de recursos do Azure

O Azure Arc para servidores (versão prévia) depende dos seguintes provedores de recursos do Azure em sua assinatura para usar esse serviço:

* **Microsoft.HybridCompute**
* **Microsoft.GuestConfiguration**

Se não estiverem registrados, você poderá registrá-los usando os seguintes comandos:

Azure PowerShell:

```azurepowershell-interactive
Login-AzAccount
Set-AzContext -SubscriptionId [subscription you want to onboard]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
```

CLI do Azure:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

Você também pode registrar os provedores de recursos no portal do Azure seguindo as etapas no [portal do Azure](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

## <a name="installation-and-configuration"></a>Instalação e configuração

Conectar computadores em seu ambiente híbrido diretamente com o Azure pode ser feito usando métodos diferentes, dependendo das suas necessidades. A tabela a seguir realça cada método para determinar qual funciona melhor para sua organização.

| Método | Descrição |
|--------|-------------|
| Interativamente | Instalar manualmente o agente em um número único ou pequeno de computadores seguindo as etapas em [Conectar computadores do portal do Azure](onboard-portal.md).<br> No portal do Azure, você pode gerar um script e executá-lo no computador para automatizar as etapas de instalação e configuração do agente.|
| Em escala | Instalar e configurar o agente para vários computadores seguindo [Conectar computadores usando uma Entidade de Serviço](onboard-service-principal.md).<br> Esse método cria uma entidade de serviço para conectar computadores de maneira não interativa.|
| Em escala | Instale e configure o agente para vários computadores seguindo o método [Como usar o DSC do Windows PowerShell](onboard-dsc.md).<br> Esse método usa uma entidade de serviço para conectar computadores de maneira não interativa com o DSC do PowerShell. |

## <a name="next-steps"></a>Próximas etapas

Para começar a avaliar o Azure Arc para servidores (versão prévia), siga o artigo [Conectar computadores híbridos ao Azure do portal do Azure](onboard-portal.md).
