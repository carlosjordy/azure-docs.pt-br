---
title: 'Início rápido: criar um pipeline de CI/CD para Ruby on Rails usando o iniciador DevOps do Azure'
description: Com o Azure DevOps Starter, é mais fácil começar a usar o Azure. É possível lançar um aplicativo Web do Ruby em um serviço do Azure em algumas etapas rápidas.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: gwallace
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: cde959d8e075b55cb6cbb37479ca49cdd8a8c0c1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82233730"
---
# <a name="create-a-cicd-pipeline-for-ruby-on-rails-by-using-azure-devops-starter"></a>Criar um pipeline de CI/CD para Ruby on Rails usando o iniciador DevOps do Azure

Configure a CI (integração contínua) e o CD (entrega contínua) para seu aplicativo Ruby on Rails usando o Azure DevOps Starter. O iniciante do DevOps simplifica a configuração inicial de um pipeline de versão e compilação DevOps do Azure.

Caso não tenha uma assinatura do Azure, você pode obter uma gratuita via [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="sign-in-to-the-azure-portal"></a>Entre no Portal do Azure

O iniciador DevOps do Azure cria um pipeline de CI/CD no Azure Repos. É possível criar uma nova organização do Azure DevOps ou usar uma existente. O DevOps Starter também cria recursos do Azure na assinatura do Azure de sua escolha.

1. Entre no [portal do Azure](https://portal.azure.com).

1. Na caixa de pesquisa, digite **DevOps Starter** e selecione. Clique em **Adicionar** para criar um.

    ![O painel do DevOps Starter](_img/azure-devops-starter-aks/search-devops-starter.png) 

## <a name="select-a-sample-app-and-azure-service"></a>Selecionar um aplicativo de exemplo e o serviço do Azure

1. Selecione o aplicativo de exemplo do **Ruby**.

1. Selecione a estrutura do aplicativo **Ruby on Rails**. Quando terminar, selecione **Avançar**.

1. **Aplicativo Web no Linux** é o destino de implantação padrão.  Como opção, você pode escolher **Aplicativo Web para Contêineres**. A estrutura do aplicativo escolhida anteriormente determina o tipo de destino de implantação do serviço do Azure disponível aqui. 
    
1. Selecione o serviço de destino de sua escolha, depois selecione **Avançar**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Configurar o Azure DevOps e uma assinatura do Azure 

1. Crie uma nova organização do Azure DevOps gratuita ou escolha uma existente. 

1. Insira um nome para o projeto do Azure DevOps. 

1. Selecione a assinatura e o local do Azure, insira um nome para o aplicativo e selecione **Concluído**.  
    Após alguns minutos, o painel inicial do DevOps é exibido no portal do Azure. Um aplicativo de exemplo é configurado em um repositório em sua organização do Azure DevOps, é executado um build, e seu aplicativo é implantado no Azure. 
    
    O painel oferece visibilidade no seu repositório de código, pipeline de CI/CD e aplicativo no Azure. À direita, selecione **Procurar** para exibir o aplicativo em execução.

    ![Exibição Painel](_img/azure-devops-project-go/dashboardnopreview.png) 

## <a name="commit-your-code-changes-and-execute-the-cicd"></a>Confirmar as alterações de código e executar a CI/CD

O iniciador DevOps do Azure cria um repositório git no Azure Pipelines ou no GitHub. Para exibir o repositório e fazer alterações de código no seu aplicativo, faça o seguinte:

1. No painel inicial do DevOps, à esquerda, selecione o link para o Branch mestre. O link abre uma exibição do repositório de Git recém-criado.

1. Para exibir a URL do clone do repositório, selecione **Clone** no canto superior direito. Você pode clonar seu repositório Git no seu IDE favorito. Nas próximas etapas, você pode usar o navegador da Web para criar e confirmar as alterações de código diretamente no branch mestre.

1. À esquerda, vá até o arquivo *app/views/pages/home.html.erb*, depois selecione **Editar**.

1. Faça uma alteração no arquivo. Por exemplo, modifique o texto dentro de uma das marcas div.

1. Selecione **Confirmar**, depois salve as alterações.

1. No navegador, acesse o painel do DevOps Starter. Um build deve estar em andamento. As alterações feitas são automaticamente compiladas e implantadas via pipeline de CI/CD.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Examinar o pipeline de CI/CD do Azure Pipelines

O Azure DevOps Starter configura automaticamente um pipeline de CI/CD completo em sua organização do DevOps do Azure. Explore e personalize o pipeline conforme necessário. Para se familiarizar com os pipelines de build e de lançamento do Azure DevOps, faça o seguinte:

1. Vá até o painel do DevOps Starter.

1. Na parte superior, selecione **Pipeline de build**. Uma guia do navegador exibe o pipeline de build do seu novo projeto.

1. Aponte para o campo **Status** e selecione as reticências (...). Um menu exibe várias opções, como o enfileiramento de um novo build, pausando um build e editando o pipeline de build.

1. Selecione **Editar**.

1. Nesse painel, é possível examinar as diversas tarefas do pipeline de build. A compilação realiza várias tarefas, como efetuar buscas das fontes no repositório Git, restaurar dependências e publicar as saídas usadas para as implantações.

1. Na parte superior do pipeline de build, selecione o nome desse pipeline.

1. Altere o nome do pipeline de build para algo mais descritivo, selecione **Salvar e enfileirar** e selecione **Salvar**.

1. Embaixo do nome do pipeline de build, selecione **Histórico**. Esse painel exibe uma trilha de auditoria das alterações recentes do build. O Azure DevOps controla quaisquer alterações feitas no pipeline de build e permite que você compare versões.

1. Selecione **Gatilhos**.  O DevOps Starter cria automaticamente um gatilho de CI, e cada confirmação no repositório inicia um novo build. Como opção, é possível incluir ou excluir os branches do processo de CI.

1. Selecione **Retenção**. Dependendo do seu cenário, você pode especificar políticas para manter ou remover um determinado número de compilações.

1. Selecione **Build e Lançamento** e selecione **Lançamentos**.  O DevOps Starter cria um pipeline de lançamento para gerenciar implantações no Azure.

1. Selecione as reticências (...) ao lado do pipeline de lançamento e selecione **Editar**. O pipeline de lançamento contém um *pipeline* que define o processo de lançamento.

1. Em **Artefatos**, selecione **Soltar**. O pipeline de build examinado anteriormente produz a saída usada para o artefato. 

1. À direita do ícone **Soltar**, selecione **Gatilho de implantação contínua**. Esse pipeline de lançamento tem um gatilho de CD habilitado, que executa uma implantação sempre que houver um novo artefato de build disponível. Outra opção é desabilitar o gatilho para que suas implantações exijam uma execução manual. 

1. À esquerda, selecione **Tarefas**. Tarefas são as atividades que seu processo de implantação realiza. Neste exemplo, uma tarefa foi criada para ser implantada no Serviço de Aplicativo do Azure.

1. À direita, selecione **Exibir versões** para exibir um histórico de versões.

1. Selecione as reticências (...) ao lado de uma versão e selecione **Abrir**. É possível explorar vários menus, como um resumo da versão, itens de trabalho associados e testes.

1. Selecione **Confirmações**. Essa exibição mostra as confirmações de código associadas a essa implantação. 

1. Selecione **Logs**. Os logs contêm informações úteis sobre o processo de implantação. É possível exibi-los durante e após as implantações.

## <a name="clean-up-resources"></a>Limpar os recursos

Quando eles não forem mais necessários, é possível excluir a instância de Serviço de Aplicativo do Azure e recursos relacionados criados neste início rápido. Para fazer isso, use a funcionalidade **Excluir** no painel do DevOps Starter.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre como modificar os pipelines de build e de lançamento para atender às necessidades de sua equipe, confira:

> [!div class="nextstepaction"]
> [Definir seu pipeline de CD (implantação contínua) de vários estágios](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
