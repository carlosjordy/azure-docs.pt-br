---
title: Criar sua primeira função usando modelos do Azure Resource Manager
description: Crie e implante no Azure uma função sem servidor simples disparada por HTTP usando um modelo de Azure Resource Manager.
ms.date: 3/5/2020
ms.topic: quickstart
ms.service: azure-functions
ms.custom: subject-armqs
ms.openlocfilehash: 403ff6407105574c78b8e600c37efbe61d2f8b79
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84740193"
---
# <a name="quickstart-create-and-deploy-azure-functions-resources-from-a-resource-manager-template"></a>Início Rápido: Criar e implantar recursos de Azure Functions de um modelo do Resource Manager

Neste artigo, você usará um modelo do Azure Resource Manager para criar uma função que responde a solicitações HTTP. 

A realização deste início rápido gera um pequeno custo de alguns centavos de dólar ou menos em sua conta do Azure. 

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Pré-requisitos

### <a name="azure-account"></a>Conta do Azure 

Antes de começar, você precisa ter uma conta do Azure com uma assinatura ativa. [Crie uma conta gratuitamente](https://azure.microsoft.com/free/).

### <a name="create-a-local-functions-project"></a>Criar um projeto local do Functions

Este artigo exigirá um projeto de código de funções locais para ser executado nos recursos do Azure que você criar. Se você não criar primeiro um projeto para publicar, não poderá concluir a seção de implantação deste artigo. 

Escolha uma das seguintes guias, siga o link e conclua a seção para criar um aplicativo de funções na linguagem de programação de sua escolha:

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[Criar seu projeto de funções locais no Visual Studio Code](functions-create-first-function-vs-code.md#create-an-azure-functions-project)

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[Criar seu projeto de funções locais no Visual Studio](functions-create-your-first-function-visual-studio.md#create-a-function-app-project)

# <a name="command-line"></a>[Linha de comando](#tab/command-line)

[Criar seu projeto de funções locais da linha de comando](functions-create-first-azure-function-azure-cli.md#create-a-local-function-project)

---

Depois de criar seu projeto localmente, você cria os recursos necessários para executar sua nova função no Azure. 

## <a name="create-a-serverless-function-app-in-azure"></a>Criar um aplicativo de funções sem servidor no Azure

### <a name="review-the-template"></a>Examinar o modelo

O modelo usado neste início rápido é proveniente dos [modelos de Início Rápido do Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic).

:::code language="json" source="~/quickstart-templates/101-function-app-create-dynamic/azuredeploy.json" :::

Os seguintes quatro recursos do Azure são criados por este modelo:

+ [**Microsoft.Storage/storageAccounts**](/azure/templates/microsoft.storage/storageaccounts): crie uma conta de Armazenamento do Azure, que é requerida pelo Functions.
+ [**Microsoft. Web/serverfarms**](/azure/templates/microsoft.web/serverfarms): criar um plano de hospedagem de consumo sem servidor para o aplicativo de funções.
+ [**Microsoft.Web/sites**](/azure/templates/microsoft.web/sites): criar um aplicativo de funções.
+ [**microsoft.insights/components**](/azure/templates/microsoft.insights/components): criar uma instância do Application Insights para monitoramento.

### <a name="deploy-the-template"></a>Implantar o modelo

# <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)
```azurecli-interactive
read -p "Enter a resource group name that is used for generating resource names:" resourceGroupName &&
read -p "Enter the location (like 'eastus' or 'northeurope'):" location &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-function-app-create-dynamic/azuredeploy.json" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri  $templateUri &&
echo "Press [ENTER] to continue ..." &&
read
```
# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter a resource group name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (like 'eastus' or 'northeurope')"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-function-app-create-dynamic/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

Read-Host -Prompt "Press [ENTER] to continue ..."
```
---

## <a name="validate-the-deployment"></a>Validar a implantação

Em seguida, publicando seu projeto no Azure e chamando o ponto de extremidade HTTP da função, valide os recursos de hospedagem do aplicativo de funções que você criou.

### <a name="publish-the-function-project-to-azure"></a>Publicar o projeto de funções no Azure

Use as seguintes etapas para publicar seu projeto para os novos recursos do Azure:

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [functions-republish-vscode](../../includes/functions-republish-vscode.md)]

Na saída, copie a URL do gatilho HTTP. Use isso para testar sua função em execução no Azure. 

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto e selecione **Publicar**.

1. Em **Escolher um destino de publicação**, escolha **Plano de Consumo do Azure Functions** com **Selecionar existente** e selecione **Criar perfil**.

    :::image type="content" source="media/functions-create-first-function-arm/choose-publish-target-visual-studio.png" alt-text="Escolher um destino de publicação existente":::

1. Escolha sua **Assinatura**, expanda o grupo de recursos, selecione seu aplicativo de funções e selecione **OK**.

1. Depois que a publicação for concluída, copie a **URL do site**.

    :::image type="content" source="media/functions-create-first-function-arm/publish-summary-site-url.png" alt-text="Copie a URL do site do resumo de publicação":::

1. Acrescente o caminho `/api/<FUNCTION_NAME>?name=Functions`, em que `<FUNCTION_NAME>` é o nome da função. A URL que chama a função de gatilho HTTP está no seguinte formato:

    `http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?name=Functions`

Você usa essa URL para testar sua função de gatilho HTTP em execução no Azure.

# <a name="command-line"></a>[Linha de comando](#tab/command-line)

Para publicar seu código local para um aplicativo de funções no Azure, use o comando `publish`:

```cmd
func azure functionapp publish <FUNCTION_APP_NAME>
```

Neste exemplo, substitua `<FUNCTION_APP_NAME>` pelo nome de seu aplicativo de funções. Talvez seja necessário entrar novamente usando `az login`. 

Na saída, copie a URL do gatilho HTTP. Use isso para testar sua função em execução no Azure.

---

### <a name="invoke-the-function-on-azure"></a>Invocar a função no Azure

Cole a URL que você copiou para a solicitação HTTP na barra de endereços do navegador, verifique se a cadeia de caracteres de consulta `name` como `?name=Functions` foi anexada ao final desta URL e, em seguida, execute a solicitação. 

Você verá uma resposta semelhante a essa:

<pre>Hello Functions!</pre>

## <a name="clean-up-resources"></a>Limpar os recursos

Se você prosseguir para a próxima etapa e adicionar uma associação de saída de fila do Armazenamento do Azure, mantenha todos os seus recursos, pois você se baseará no que já fez.

Caso contrário, use o comando a seguir para excluir o grupo de recursos e todos os recursos contidos nele para evitar custos adicionais.

```azurecli
az group delete --name <RESOURCE_GROUP_NAME>
```

Substitua `<RESOURCE_GROUP_NAME>` pelo nome do grupo de recursos.

## <a name="next-steps"></a>Próximas etapas

Agora que você publicou sua primeira função, saiba mais adicionando uma associação de saída à sua função.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

> [!div class="nextstepaction"]
> [Conectar-se a uma fila do Armazenamento do Azure](functions-add-output-binding-storage-queue-vs-code.md)

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

> [!div class="nextstepaction"]
> [Conectar-se a uma fila do Armazenamento do Azure](functions-add-output-binding-storage-queue-vs.md)

# <a name="command-line"></a>[Linha de comando](#tab/command-line)

> [!div class="nextstepaction"]
> [Conectar-se a uma fila do Armazenamento do Azure](functions-add-output-binding-storage-queue-cli.md)

---
