---
title: Microsoft Teams na área de trabalho virtual do Windows – Azure
description: Como usar o Microsoft Teams na área de trabalho virtual do Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/29/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: baab0160247e17556f0928f12f26a5ecca767210
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87129297"
---
# <a name="use-microsoft-teams-on-windows-virtual-desktop"></a>Usar o Microsoft Teams na área de trabalho virtual do Windows

>[!IMPORTANT]
>A otimização de mídia para o Microsoft Teams está atualmente em visualização pública. É recomendável avaliar a experiência do usuário das equipes otimizadas antes de implantar equipes para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.

>[!NOTE]
>A otimização de mídia para o Microsoft Teams está disponível somente para o cliente de desktop do Windows em computadores Windows 10. As otimizações de mídia exigem o cliente da área de trabalho do Windows versão 1.2.1026.0 ou posterior.

O Microsoft Teams na área de trabalho virtual do Windows dá suporte a chat e colaboração. Com otimizações de mídia, ele também dá suporte à chamada e à funcionalidade de reunião. Para saber mais sobre como usar o Microsoft Teams em ambientes de VDI (Virtual Desktop Infrastructure), confira [equipes para infraestrutura de área de trabalho virtualizada](/microsoftteams/teams-for-vdi/).

Com a otimização de mídia para o Microsoft Teams, o cliente de área de trabalho do Windows manipula áudio e vídeo localmente para chamadas e reuniões de equipes. Você ainda pode usar o Microsoft Teams na área de trabalho virtual do Windows com outros clientes sem chamadas e reuniões otimizadas. Os recursos de chat e colaboração de equipes têm suporte em todas as plataformas. Para redirecionar dispositivos locais em sua sessão remota, confira [Personalizar propriedades de protocolo RDP para um pool de hosts](#customize-remote-desktop-protocol-properties-for-a-host-pool).

## <a name="prerequisites"></a>Pré-requisitos

Antes de poder usar o Microsoft Teams na área de trabalho virtual do Windows, você precisará fazer o seguinte:

- [Prepare sua rede para o](/microsoftteams/prepare-network/) Microsoft Teams.
- Instale o [cliente de área de trabalho do Windows](connect-windows-7-10.md) em um dispositivo Windows 10 ou Windows 10 IOT Enterprise que atenda aos requisitos de hardware do Microsoft Team [para equipes em um computador Windows](/microsoftteams/hardware-requirements-for-the-teams-app#hardware-requirements-for-teams-on-a-windows-pc/).
- Conecte-se a uma VM (máquina virtual) do Windows 10 de várias sessões ou do Windows 10 Enterprise.
- Instale o aplicativo de área de trabalho de equipes no host usando a instalação por máquina. A otimização de mídia para o Microsoft Teams requer o aplicativo de área de trabalho de equipes versão 1.3.00.4461 ou posterior.

## <a name="install-the-teams-desktop-app"></a>Instalar o aplicativo de área de trabalho de equipes

Esta seção mostrará a você como instalar o aplicativo de área de trabalho de equipes em sua imagem de VM do Windows 10 de várias sessões ou de sua máquina virtual. Para saber mais, confira [instalar ou atualizar o aplicativo de área de trabalho das equipes no VDI](/microsoftteams/teams-for-vdi#install-or-update-the-teams-desktop-app-on-vdi/).

### <a name="prepare-your-image-for-teams"></a>Preparar sua imagem para equipes

Para habilitar a otimização de mídia para equipes, defina a seguinte chave do registro no host:

1. No menu Iniciar, execute **regedit** como administrador. Navegue até **HKEY_LOCAL_MACHINE \software\microsoft\teams**.
2. Crie o seguinte valor para a chave de equipes:

| Nome             | Tipo   | Dados/valor  |
|------------------|--------|-------------|
| IsWVDEnvironment | DWORD  | 1           |

### <a name="install-the-teams-websocket-service"></a>Instalar o serviço WebSocket de equipes

Instale o [serviço WebSocket](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4yj0i) em sua imagem de VM. Se você encontrar um erro de instalação, instale os [Microsoft Visual C++ redistribuíveis mais recentes](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) e tente novamente.

#### <a name="latest-websocket-service-versions"></a>Versões mais recentes do serviço WebSocket

A tabela a seguir lista as versões atuais disponíveis para cada grupo de usuários:

|Versão    |Data de liberação  |
|-----------|--------------|
|0.11.0     |05/29/2020    |

### <a name="install-microsoft-teams"></a>Instalar o Microsoft Teams

Você pode implantar o aplicativo de área de trabalho de equipes usando uma instalação por computador ou por usuário. Para instalar o Microsoft Teams em seu ambiente de área de trabalho virtual do Windows:

1. Baixe o [pacote MSI das equipes](/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm/) que corresponde ao seu ambiente. É recomendável usar o instalador de 64 bits em um sistema operacional de 64 bits.

      > [!NOTE]
      > A otimização de mídia para o Microsoft Teams requer o aplicativo de área de trabalho de equipes versão 1.3.00.4461 ou posterior.

2. Execute um dos seguintes comandos para instalar o MSI na VM do host:

    - Instalação por usuário

        ```powershell
        msiexec /i <path_to_msi> /l*v <install_logfile_name> ALLUSERS=1
        ```

        Esse processo é a instalação padrão, que instala equipes na pasta **% AppData%** User. As equipes não funcionarão corretamente com a instalação por usuário em uma configuração não persistente.

    - Instalação por máquina

        ```powershell
        msiexec /i <path_to_msi> /l*v <install_logfile_name> ALLUSER=1 ALLUSERS=1
        ```

        Isso instala as equipes na pasta arquivos de programas (x86) em um sistema operacional de 64 bits e na pasta arquivos de programas em um sistema operacional de 32 bits. Neste ponto, a configuração da imagem dourada foi concluída. A instalação de equipes por máquina é necessária para configurações não persistentes.

        Na próxima vez que abrir as equipes em uma sessão, você será solicitado a fornecer suas credenciais.

        > [!NOTE]
        > Os usuários e administradores não podem desabilitar o lançamento automático para equipes durante a entrada no momento.

3. Para desinstalar o MSI da VM do host, execute este comando:

      ```powershell
      msiexec /passive /x <msi_name> /l*v <uninstall_logfile_name>
      ```

      Isso desinstala as equipes da pasta arquivos de programas (x86) ou dos arquivos de programas, dependendo do ambiente do sistema operacional.

      > [!NOTE]
      > Quando você instala equipes com a configuração de MSI usuário = 1, as atualizações automáticas serão desabilitadas. Recomendamos que você atualize as equipes pelo menos uma vez por mês. Para saber mais sobre como implantar o aplicativo de área de trabalho das equipes, confira [implantar o aplicativo de área de trabalho das equipes na VM](/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm/).

### <a name="verify-media-optimizations-loaded"></a>Verificar otimizações de mídia carregadas

Depois de instalar o serviço WebSocket e o aplicativo de área de trabalho das equipes, siga estas etapas para verificar se as otimizações de mídia de equipes foram carregadas:

1. Selecione sua imagem de perfil de usuário e, em seguida, selecione **sobre**.
2. Selecione a **versão**.

      Se as otimizações de mídia forem carregadas, a faixa mostrará a você a **WVD de mídia otimizada**. Se a faixa mostrar que a **mídia WVD não está conectada**, saia do aplicativo de equipes e tente novamente.

3. Selecione sua imagem de perfil de usuário e, em seguida, selecione **configurações**.

      Se as otimizações de mídia estiverem carregadas, os dispositivos de áudio e câmeras disponíveis localmente serão enumerados no menu do dispositivo. Se o menu Mostrar **áudio remoto**, saia do aplicativo de equipes e tente novamente. Se os dispositivos ainda não aparecerem no menu, volte para [instalar o Microsoft Teams](#install-microsoft-teams) e verifique se você concluiu o processo de instalação.

## <a name="known-issues-and-limitations"></a>Limitações e problemas conhecidos

O uso de equipes em um ambiente virtualizado é diferente do uso de equipes em um ambiente não virtualizado. Para obter mais informações sobre as limitações de equipes em ambientes virtualizados, confira [equipes para infraestrutura de área de trabalho virtualizada](/microsoftteams/teams-for-vdi#known-issues-and-limitations/).

### <a name="client-deployment-installation-and-setup"></a>Implantação, instalação e instalação do cliente

- Com a instalação por máquina, as equipes no VDI não são atualizadas automaticamente da mesma forma que os clientes de equipes não VDI. Para atualizar o cliente, você precisará atualizar a imagem da VM instalando um novo MSI.
- Atualmente, as equipes mostram apenas o fuso horário UTC em chat, canais e calendário.
- A otimização de mídia para equipes só tem suporte para o cliente de desktop do Windows em computadores que executam o Windows 10.
- Não há suporte para o uso de proxies HTTP explícitos definidos em um ponto de extremidade.

### <a name="calls-and-meetings"></a>Chamadas e reuniões

- O cliente do teams desktop em ambientes de área de trabalho virtual do Windows não dá suporte a eventos ao vivo. Por enquanto, é recomendável unir eventos ao vivo do [cliente Web Teams](https://teams.microsoft.com) em sua sessão remota em vez disso.
- Minimizar o aplicativo de equipes durante uma chamada ou reunião pode fazer com que o feed de vídeo de entrada se desapareça quando você expandir o aplicativo.
- Chamadas ou reuniões atualmente não dão suporte ao compartilhamento de aplicativos. As sessões de desktop dão suporte ao compartilhamento de desktop.
- Quando o compartilhamento de área de trabalho em uma configuração de vários monitores, todos os monitores são compartilhados.
- Não há suporte para dar controle e assumir controle no momento.
- As equipes na área de trabalho virtual do Windows só dão suporte a uma entrada de vídeo de entrada por vez. Isso significa que sempre que alguém tentar compartilhar sua tela, a tela será exibida em vez da tela de líder da reunião.
- Devido a limitações de WebRTC, a resolução de fluxo de vídeo de entrada e saída é limitada a 720p.
- O aplicativo Teams não dá suporte a botões HID ou controles de LED com outros dispositivos.

Para problemas conhecidos de equipes que não estão relacionados a ambientes virtualizados, consulte [equipes de suporte em sua organização](/microsoftteams/known-issues/)

## <a name="uservoice-site"></a>Site UserVoice

Forneça comentários para o Microsoft Teams na área de trabalho virtual do Windows no [site do UserVoice](https://microsoftteams.uservoice.com/)das equipes.

## <a name="collect-teams-logs"></a>Coletar logs de equipes

Se você encontrar problemas com o aplicativo de área de trabalho de equipes em seu ambiente de área de trabalho virtual do Windows, colete logs do cliente em **% AppData% \Microsoft\Teams\logs.txt** na VM do host.

Se você encontrar problemas com chamadas e reuniões, colete os logs do cliente da Web da equipe com a combinação de teclas **Ctrl**  +  **ALT**  +  **Shift**  +  **1**. Os logs serão gravados no **log de diagnóstico do%userprofile%\Downloads\MSTeams DATE_TIME.txt** na VM do host.

## <a name="contact-microsoft-teams-support"></a>Contate o suporte do Microsoft Teams

Para entrar em contato com o suporte do Microsoft Teams, vá para o [centro de administração do Microsoft 365](/microsoft-365/admin/contact-support-for-business-products).

## <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Personalizar propriedades de protocolo RDP para um pool de hosts

Personalizar as propriedades de protocolo RDP (RDP) de um pool de hosts, como a experiência de vários monitores ou habilitar o redirecionamento de microfone e áudio, permite que você forneça uma experiência ideal para seus usuários com base em suas necessidades.

Habilitar redirecionamentos de dispositivo não é necessário ao usar equipes com otimização de mídia. Se você estiver usando equipes sem otimização de mídia, defina as seguintes propriedades de RDP para habilitar o redirecionamento de microfone e câmera:

- `audiocapturemode:i:1`habilita a captura de áudio do dispositivo local e redireciona os aplicativos de áudio na sessão remota.
- `audiomode:i:0`Reproduz áudio no computador local.
- `camerastoredirect:s:*`redireciona todas as câmeras.

Para saber mais, confira [Personalizar propriedades de protocolo RDP para um pool de hosts](customize-rdp-properties.md).
