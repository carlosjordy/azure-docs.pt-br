---
title: Como importar e exportar Azure AD Connect definições de configuração
description: Este documento descreve as perguntas frequentes sobre o provisionamento de nuvem.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/13/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a94d356cb3c0345f575b4b5a44aa7f228535e66d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87019872"
---
# <a name="importing-and-exporting-azure-ad-connect-configuration-settings-public-preview"></a>Importando e exportando definições de configuração de Azure AD Connect (visualização pública) 

Azure AD Connect implantações variam de uma instalação de modo expresso de floresta única, para implantações complexas que são sincronizadas em várias florestas usando regras de sincronização personalizadas. Devido ao grande número de opções e mecanismos de configuração, é essencial entender quais configurações estão em vigor e ser capaz de implantar rapidamente um servidor com uma configuração idêntica. Esse recurso apresenta a capacidade de catalogar a configuração de um determinado servidor de sincronização e importar as configurações para uma nova implantação. Instantâneos de configurações de sincronização diferentes podem ser comparados para visualizar facilmente as diferenças entre dois servidores ou o mesmo servidor ao longo do tempo. 

Cada vez que a configuração é alterada no assistente de Azure AD Connect, um novo arquivo de configurações JSON com carimbo de data/hora é exportado automaticamente para **%ProgramData%\AADConnect**. O nome do arquivo de configurações está no formato **aplicado-SynchronizationPolicy-*. JSON** em que a última parte do nome do arquivo é um carimbo de data/hora. 

> [!IMPORTANT]
> Somente as alterações feitas por Azure AD Connect são exportadas automaticamente. Todas as alterações feitas usando o PowerShell, o Synchronization Service Manager ou o editor de regras de sincronização devem ser exportadas sob demanda conforme necessário para manter uma cópia atualizada. A exportação sob demanda também pode ser usada para fazer uma cópia das configurações em um local seguro para fins de recuperação de desastre. 

## <a name="exporting-azure-ad-connect-settings"></a>Exportando configurações de Azure AD Connect 

Para exibir um resumo de suas definições de configuração, abra a ferramenta Azure AD Connect e escolha a tarefa adicional denominada: exibir ou exportar a configuração atual. Um resumo rápido de suas configurações é mostrado junto com a capacidade de exportar a configuração completa do servidor. 

Por padrão, as configurações serão exportadas para **%ProgramData%\AADConnect**, no entanto, você pode optar por salvar as configurações em um local protegido para garantir a disponibilidade em caso de desastre. As configurações são exportadas usando o formato de arquivo JSON e não devem ser criadas ou editadas para garantir a consistência lógica. Importação manual criada ou arquivo editado não tem suporte e pode levar a resultados inesperados. 

## <a name="importing-azure-ad-connect-settings"></a>Importando configurações de Azure AD Connect 

Para importar configurações exportadas anteriormente, faça o seguinte:
 
1. Instale o **Azure ad Connect** em um novo servidor. 
2. Selecione a opção **Personalizar** após a página de **boas-vindas** . 
3. Clique em **Importar configurações de sincronização**. Procure o arquivo de configurações JSON exportado anteriormente.  
4. Clique em **Instalar**.

![Instalar componentes](media/how-to-connect-import-export-config/import1.png)

> [!NOTE]
> Substitua as configurações nesta página, como o uso de SQL Server em vez do LocalDB ou o uso da conta de serviço existente em vez do VSA padrão, etc. Essas configurações não são importadas do arquivo de definições de configuração, mas existem para fins de comparação e informações.

### <a name="import-installation-experience"></a>Importar experiência de instalação 

A experiência de importação da instalação é mantida intencionalmente simples com entradas mínimas do usuário para facilitar o reprodução de um servidor existente.  

Aqui estão as únicas alterações que podem ser feitas durante a experiência de instalação. Todas as outras alterações podem ser feitas após a instalação do assistente de Azure AD Connect.: 
- **Azure Active Directory credenciais**   – o nome da conta para o administrador global do Azure usado para configurar o servidor original é sugerido por padrão e **deve**   ser alterado se você quiser sincronizar informações em um novo diretório.
- **Entrada do usuário**   – as opções de logon configuradas para o servidor original são selecionadas por padrão e solicitarão automaticamente as credenciais ou outras informações necessárias durante a configuração. Em casos raros, pode haver a necessidade de configurar um servidor com opções diferentes para evitar a alteração do comportamento do servidor ativo. Caso contrário, basta pressionar avançar para usar as mesmas configurações. 
- Credenciais do diretório **local**   – para cada diretório local incluído nas configurações de sincronização, você deve fornecer credenciais para criar uma conta de sincronização ou fornecer uma conta de sincronização personalizada criada previamente. Esse procedimento é idêntico à experiência de instalação limpa com a exceção que você não pode adicionar ou remover diretórios. 
- **Opções**   de configuração – assim como com uma instalação limpa, você pode optar por definir as configurações iniciais para iniciar a sincronização automática ou habilitar o modo de preparo. A principal diferença é que o modo de preparo é intencionalmente habilitado por padrão para permitir a comparação dos resultados de configuração e sincronização antes de exportar ativamente os resultados para o Azure. 

![Conectar diretórios](media/how-to-connect-import-export-config/import2.png)

> [!NOTE]
> Somente um servidor de sincronização pode estar na função primária e exportar ativamente as alterações de configuração para o Azure. Todos os outros servidores devem ser colocados no modo de preparo. 

## <a name="migrating-settings-from-an-existing-server"></a>Migrando configurações de um servidor existente 

Se um servidor existente não oferecer suporte ao gerenciamento de configurações, você poderá optar por atualizar o servidor in-loco ou migrar as configurações para uso em um novo servidor de preparo.  

A migração requer a execução de um script do PowerShell que extrai as configurações existentes para uso em uma nova instalação.Esse método é recomendado para catalogar as configurações do servidor existente e, em seguida, aplicá-las a um servidor de preparo recém-instalado.A comparação das configurações do servidor original para o servidor recém-criado visualizará rapidamente as alterações entre os servidores.Como sempre, siga o processo de certificação da sua organização para garantir que nenhuma configuração adicional seja necessária.  

### <a name="migration-process"></a>Processo de migração 
Para migrar as configurações, faça o seguinte:

1. Inicie o **AzureADConnect.msi** no novo servidor de preparo e pare na página de boas-vindas do Azure ad Connect.

2. Copie **MigrateSettings.ps1** do diretório Microsoft Azure ad Connect\Tools para um local no servidor existente.  Por exemplo, C:\setup.  Em que instalação é um diretório que foi criado no servidor existente.

   ![Conectar diretórios](media/how-to-connect-import-export-config/migrate1.png)

3. Execute o script conforme mostrado abaixo e salve todo o diretório de configuração de servidor de nível inferior. Copie esse diretório para o novo servidor de preparo. Observe que você precisa copiar toda a pasta **exported-ServerConfiguration-*** para o novo servidor.

   ![Conectar diretórios ](media/how-to-connect-import-export-config/migrate2.png)
    ![ conectar diretórios](media/how-to-connect-import-export-config/migrate3.png)

5. Inicie **Azure ad Connect** clicando duas vezes no ícone na área de trabalho. Aceitar o EULA, na próxima página, clique no botão **Personalizar** . 
6. Selecione a caixa de seleção **Importar configurações de sincronização** e clique no botão **procurar** para procurar a pasta copiada sobre exportada-ServerConfiguration-* e selecione a MigratedPolicy.jsem para importar as configurações migradas.

   ![Conectar diretórios](media/how-to-connect-import-export-config/migrate4.png)

7. Para comparar as configurações migradas com as configurações aplicadas, procure as mais recentes **migrated-SynchronizationPolicy-*. JSON** e **aplicado-SynchronizationPolicy-*. JSON** (* é o carimbo de data/hora) em **%ProgramData%\AADConnect**. Use sua ferramenta de comparação de arquivo favorita para comparar a paridade. 

## <a name="post-installation-verification"></a>Verificação pós-instalação 

A comparação do arquivo de configurações originalmente importado, com o arquivo de configurações exportado, do servidor implantado recentemente é uma etapa essencial na compreensão de quaisquer diferenças entre a pretendida, em comparação com a implantação resultante. Usar seu aplicativo de comparação de texto lado a lado favorito produz uma visualização instantânea que realça rapidamente as alterações desejadas ou acidentais. Embora muitas das etapas de configuração manual já sejam eliminadas, você ainda deve seguir o processo de certificação da sua organização para garantir que nenhuma configuração adicional seja necessária. Essa configuração pode ocorrer se você aproveitar as configurações avançadas, que não são capturadas no momento na versão de visualização pública do gerenciamento de configurações. 

As limitações conhecidas incluem o seguinte: 
- **Regras**   de sincronização – a precedência de uma regra personalizada deve estar no intervalo reservado de 0-99 para evitar conflitos com as regras padrão da Microsoft. Colocar uma regra personalizada fora do intervalo reservado pode fazer com que a regra personalizada seja deslocada conforme as regras padrão são adicionadas à configuração. Um problema semelhante ocorrerá se sua configuração contiver regras padrão modificadas. A modificação de uma regra padrão é altamente desencorajada e o posicionamento da regra provavelmente estará incorreto. 
- **Write-back**   do dispositivo – essas configurações são catalogadas no momento em que não são aplicadas durante a configuração. Se o Write-back do dispositivo tiver sido habilitado para o servidor original, você deverá configurar manualmente o recurso no servidor implantado recentemente. 
- Tipos de objeto **sincronizados**   – Embora seja possível restringir a lista de tipos de objetos sincronizados (usuários, contatos, grupos, etc.) usando o Synchronization Service Manager, atualmente, não há suporte para esse recurso por meio das configurações de sincronização. Depois de concluir a instalação, você deve reaplicar manualmente a configuração avançada. 
- Perfis de execução **personalizados**   -Embora seja possível modificar o conjunto padrão de perfis de execução usando o Synchronization Service Manager, atualmente, não há suporte para esse recurso por meio das configurações de sincronização. Depois de concluir a instalação, você deve reaplicar manualmente a configuração avançada. 
- **Configurando a hierarquia**   de provisionamento – Esse recurso avançado do Synchronization Service Manager não tem suporte por meio de configurações de sincronização e deve ser reconfigurado manualmente após a conclusão da implantação inicial. 
- Autenticação AD FS e **PingFederate**   – os métodos de logon associados a esses recursos de autenticação serão previamente selecionados, no entanto, você deve fornecer de forma interativa todos os outros parâmetros de configuração necessários. 
- **Uma regra de sincronização personalizada desabilitada será importada como habilitada** Uma regra de sincronização personalizada desabilitada será importada como habilitada. Certifique-se de desabilitá-lo no novo servidor também.

 ## <a name="next-steps"></a>Próximas etapas

- [Pré-requisitos e hardware](how-to-connect-install-prerequisites.md) 
- [Configurações expressas](how-to-connect-install-express.md)
- [Configurações personalizadas](how-to-connect-install-custom.md)
- [Instalar agentes do Azure AD Connect Health](how-to-connect-health-agent-install.md) 
