---
title: Conectar-se à área de trabalho virtual do Windows Windows 10 ou 7-Azure
description: Como se conectar à área de trabalho virtual do Windows usando o cliente de área de trabalho do Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 07/16/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 1f813d0ada516f6090b97e5858cefab110636f90
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077593"
---
# <a name="connect-with-the-windows-desktop-client"></a>Conectar-se ao Cliente de Área de Trabalho do Windows

> Aplica-se a: Windows 7, Windows 10 e Windows 10 IoT Enterprise

>[!IMPORTANT]
>Este conteúdo se aplica à atualização da Spring 2020 com objetos da Área de Trabalho Virtual do Windows do Azure Resource Manager. Se você estiver usando a Área de Trabalho Virtual do Windows na versão 2019, sem objetos do Azure Resource Manager, confira [este artigo](./virtual-desktop-fall-2019/connect-windows-7-10-2019.md).
>
> A atualização 2020 da Área de Trabalho Virtual do Windows está em versão prévia pública no momento. Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendamos usá-la para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Você pode acessar os recursos da área de trabalho virtual do Windows em dispositivos com Windows 7, Windows 10 e Windows 10 IoT Enterprise usando o cliente de área de trabalho do Windows. O cliente não dá suporte à janela 8 ou Windows 8.1.

>[!NOTE]
>O cliente do Windows padroniza automaticamente para a versão de área de trabalho virtual do Windows no outono 2019. No entanto, se o cliente detectar que o usuário também tem recursos de Azure Resource Manager, ele adicionará automaticamente os recursos ou notificará o usuário de que eles estão disponíveis.

> [!IMPORTANT]
> A Área de Trabalho Virtual do Windows não dá suporte ao cliente de RADC (Conexões de RemoteApp e Área de Trabalho) nem ao cliente de Conexão de Área de Trabalho Remota (MSTSC).

> [!IMPORTANT]
> No momento, a Área de Trabalho Virtual do Windows não dá suporte ao cliente da Área de Trabalho Remota da Microsoft Store.

## <a name="install-the-windows-desktop-client"></a>Instalar o cliente de área de trabalho do Windows

Escolha o cliente que corresponda à sua versão do Windows:

- [Windows 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32 bits](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

É possível instalar o cliente do usuário atual, que não requer direitos de administrador, ou seu administrador pode instalar e configurar o cliente para que todos os usuários no dispositivo possam acessá-lo.

Depois de instalado, o cliente pode ser iniciado no menu Iniciar pesquisando **Área de Trabalho Remota**.

## <a name="subscribe-to-a-workspace"></a>Assinar um Workspace

Há duas maneiras de assinar um workspace. O cliente pode tentar descobrir os recursos disponíveis para você por meio da sua conta corporativa ou de estudante, ou, caso ele não consiga encontrá-los, você pode especificar diretamente a URL onde os recursos estão. Depois da assinatura de um workspace, é possível iniciar recursos com um dos seguintes métodos:

- Acesse a Central de Conexão e clique duas vezes em um recurso para iniciá-lo.
- Também é possível acessar o menu Iniciar e procurar uma pasta com o nome do Workspace ou inserir o nome do recurso na barra de pesquisa.

### <a name="subscribe-with-a-user-account"></a>Assinar com uma conta de usuário

1. Na página principal do cliente, selecione **assinar**.
2. Entre com sua conta quando solicitado.
3. Os recursos aparecerão no centro de conexões e serão agrupados por espaço de trabalho.

### <a name="subscribe-with-a-url"></a>Assinar com uma URL

1. Na página principal do cliente, selecione **assinar com a URL**.
2. Insira a URL do workspace ou o endereço de email:
   - Se você usar a **URL do workspace**, use aquela que o administrador lhe forneceu. Se estiver acessando recursos da Área de Trabalho Virtual do Windows, será possível usar uma das seguintes URLs:
     - Área de Trabalho Virtual do Windows no outono de 2019: `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Área de Trabalho Virtual do Windows na primavera de 2020: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
   - Se você estiver usando o campo **email** , insira seu endereço de email. Isso instrui o cliente a procurar uma URL associada ao seu endereço de email se seu administrador tiver configurado a [descoberta de email](/windows-server/remote/remote-desktop-services/rds-email-discovery).
3. Selecione **Avançar**.
4. Entre com sua conta quando solicitado.
5. Os recursos devem aparecer no centro de conexões, agrupados por espaço de trabalho.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre como usar o cliente de área de trabalho do Windows, confira [introdução ao cliente de desktop do Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/).
