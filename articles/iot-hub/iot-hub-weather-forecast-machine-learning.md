---
title: Previsão do tempo usando o Azure Machine Learning com dados do Hub IoT
description: Use o Azure Machine Learning para prever a possibilidade de chuva com base nos dados de temperatura e umidade coletados pelo Hub IoT de um sensor.
author: robinsh
manager: philmea
keywords: Machine Learning de previsão do tempo
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/10/2020
ms.author: robinsh
ms.openlocfilehash: 5551655843b8d3ed5b6d70f5d6ed3a0eb4d0e92f
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83746968"
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Previsão do tempo usando os dados do sensor do Hub IoT no Azure Machine Learning

![Diagrama de ponta a ponta](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

O Machine Learning é uma técnica da ciência de dados que ajuda os computadores a aprenderem com os dados existentes para prever tendências, resultados e comportamentos futuros. O Azure Machine Learning é um serviço de análise preditiva na nuvem que permite criar rapidamente modelos preditivos e implantá-los como soluções de análise.

## <a name="what-you-learn"></a>O que você aprenderá

Saiba como usar o Azure Machine Learning para fazer uma previsão do tempo (possibilidade de chuva) usando os dados de temperatura e umidade do Hub IoT do Azure. A possibilidade de chuva é o resultado de um modelo de previsão de clima preparado. O modelo se baseia em dados históricos para prever a possibilidade de chuva com base na temperatura e na umidade.

## <a name="what-you-do"></a>O que fazer

- Implante o modelo de previsão do tempo como um serviço Web.
- Preparar seu Hub IoT para acesso a dados, adicionando um grupo de consumidores.
- Crie um trabalho do Stream Analytics e configure o trabalho para:
  - Leia dados de temperatura e umidade no Hub IoT.
  - Chame o serviço Web para obter a possibilidade de chuva.
  - Salve o resultado em um armazenamento de blobs do Azure.
- Use o Gerenciador de Armazenamento do Microsoft Azure para exibir a previsão do tempo.

## <a name="what-you-need"></a>O que você precisa

- Conclua o tutorial do [simulador online Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md) ou um dos tutoriais do dispositivo; por exemplo, [Raspberry Pi com Node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Eles cobrem os seguintes requisitos:
  - Uma assinatura ativa do Azure.
  - Um hub IoT do Azure em sua assinatura.
  - O aplicativo cliente que envia mensagens para o hub IoT do Azure.
- Uma conta [Azure Machine Learning Studio (clássico)](https://studio.azureml.net/).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Implantar o modelo de previsão do tempo como um serviço Web

Nesta seção, você obtém o modelo previsão do tempo da biblioteca de IA do Azure. Em seguida, você adiciona um módulo R-script ao modelo para limpar os dados de temperatura e umidade. Por fim, você implantará o modelo como um serviço Web preditivo.

### <a name="get-the-weather-prediction-model"></a>Obter o modelo de previsão do tempo

Nesta seção, você obtém o modelo previsão do tempo do Galeria de IA do Azure e o abre no Azure Machine Learning Studio (clássico).

1. Acesse a [página do modelo de previsão do tempo](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).

   ![Abra a página do modelo de previsão do tempo na Galeria de IA do Azure](media/iot-hub-weather-forecast-machine-learning/weather-prediction-model-in-azure-ai-gallery.png)

1. Clique em **Abrir no Studio (clássico)** no Microsoft Azure Machine Learning Studio (clássico).

   ![Abrir o modelo de previsão do tempo no Azure Machine Learning Studio (clássico)](media/iot-hub-weather-forecast-machine-learning/open-ml-studio.png)

### <a name="add-an-r-script-module-to-clean-temperature-and-humidity-data"></a>Adicionar um módulo R-script para limpar os dados de temperatura e umidade

Para que o modelo se comporte corretamente, os dados de temperatura e umidade devem ser conversíveis em dados numéricos. Nesta seção, você adiciona um módulo R-script ao modelo previsão do tempo que remove as linhas que têm valores de dados para temperatura ou umidade que não podem ser convertidas em valores numéricos.

1. No lado esquerdo da janela Azure Machine Learning Studio, clique na seta para expandir o painel de ferramentas. Insira "Executar" na caixa de pesquisa. Selecione o módulo **Executar R-script**.

   ![Selecione o módulo Executar R-script](media/iot-hub-weather-forecast-machine-learning/select-r-script-module.png)

1. Arraste o módulo **Executar R-script** próximo ao módulo **Limpar dados ausentes** e ao módulo **Executar R-script** no diagrama. Exclua a conexão entre os módulos **Limpar dados ausentes** e os **Executar R-script** e, em seguida, conecte as entradas e saídas do novo módulo, conforme mostrado.

   ![Adicione o módulo Executar R-script](media/iot-hub-weather-forecast-machine-learning/add-r-script-module.png)

1. Selecione o módulo novo **Executar R-script** para abrir a janela Propriedades. Copie e cole o seguinte código na caixa **R Script**.

   ```r
   # Map 1-based optional input ports to variables
   data <- maml.mapInputPort(1) # class: data.frame

   data$temperature <- as.numeric(as.character(data$temperature))
   data$humidity <- as.numeric(as.character(data$humidity))

   completedata <- data[complete.cases(data), ]

   maml.mapOutputPort('completedata')

   ```

   Quando tiver terminado, a janela Propriedades deverá ser semelhante ao seguinte:

   ![Adicione o código ao módulo Executar R-Script](media/iot-hub-weather-forecast-machine-learning/add-code-to-module.png)

### <a name="deploy-predictive-web-service"></a>Implantar um serviço Web preditivo

Nesta seção, você valida o modelo, configura um serviço Web preditivo com base no modelo e, em seguida, implanta o serviço Web.

1. Clique em **Executar** para validar as etapas no modelo. A conclusão dessa etapa pode levar alguns minutos.

   ![Execute o experimento para validar as etapas](media/iot-hub-weather-forecast-machine-learning/run-experiment.png)

1. Clique em **CONFIGURAR SERVIÇO WEB** > **Serviço Web Preditivo**. O diagrama de experimento preditivo é aberto.

   ![Implante o modelo de previsão do tempo no Azure Machine Learning Studio (clássico)](media/iot-hub-weather-forecast-machine-learning/predictive-experiment.png)

1. No diagrama de experimento preditivo, exclua a conexão entre o módulo **Entrada do serviço Web** e o **Conjunto de dados do clima** na parte superior. Em seguida, arraste o módulo **Entrada do serviço Web** em algum lugar próximo ao módulo **Modelo de Pontuação** e conecte-o conforme mostrado:

   ![Conectar dois módulos no Azure Machine Learning Studio (clássico)](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)

1. Clique em **EXECUTAR** para validar as etapas no modelo.

1. Clique em **IMPLANTAR SERVIÇO WEB** para implantar o modelo como um serviço Web.

1. No painel do modelo, baixe o **Excel 2010 ou a pasta de trabalho anterior** de **SOLICITAÇÃO/RESPOSTA**.

   > [!Note]
   > Assegure-se de baixar o **Excel 2010 ou a pasta de trabalho anterior** mesmo se estiver executando uma versão posterior do Excel no computador.

   ![Baixar o Excel para obter o ponto de extremidade SOLICITAÇÃO/RESPOSTA](media/iot-hub-weather-forecast-machine-learning/download-workbook.png)

1. Abra a pasta de trabalho do Excel, anote a **URL DO SERVIÇO WEB** e a **TECLA DE ACESSO**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Criar, configurar e executar um trabalho do Stream Analytics

### <a name="create-a-stream-analytics-job"></a>Criar um trabalho de Stream Analytics

1. No [Portal do Azure](https://portal.azure.com/), clique em **Criar um recurso** > **Internet das Coisas** > **Trabalho do Stream Analytics**.
1. Insira as seguintes informações para o trabalho.

   **Nome do trabalho**: O nome do trabalho. O nome deve ser globalmente exclusivo.

   **Grupo de recursos**: Use o mesmo grupo de recursos usado pelo hub IoT.

   **Localização**: Use o mesmo local do grupo de recursos.

   **Fixar no painel**: Marque essa opção para facilitar o acesso ao hub IoT no painel.

   ![Criar um trabalho do Stream Analytics no Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Clique em **Criar**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Adicionar uma entrada ao trabalho do Stream Analytics

1. Abra o trabalho do Stream Analytics.
1. Em **Topologia do Trabalho**, clique em **Entradas**.
1. No **entradas** painel, clique em **adicionar**e, em seguida, insira as seguintes informações:

   **Alias de entrada**: O alias exclusivo para a entrada.

   **Fonte**: Selecione **Hub IoT**.

   **Grupo de consumidores**: Selecione o grupo de consumidores criado.

   ![Adicionar uma entrada ao trabalho do Stream Analytics no Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Clique em **Criar**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Adicionar uma saída ao trabalho do Stream Analytics

1. Em **Topologia do Trabalho**, clique em **Saídas**.
1. No **saídas** painel, clique em **adicionar**e, em seguida, insira as seguintes informações:

   **Alias de saída**: o alias exclusivo para a saída.

   **Coletor**: Selecione **Armazenamento de Blobs**.

   **Conta de armazenamento**: A conta de armazenamento do armazenamento de blobs. Você pode criar uma conta de armazenamento ou usar uma existente.

   **Contêiner**: O contêiner em que o blob foi salvo. Você pode criar um contêiner ou usar um existente.

   **Formato de serialização do evento**: Selecione **CSV**.

   ![Adicionar uma saída ao trabalho do Stream Analytics no Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Clique em **Criar**.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Adicionar uma função ao trabalho do Stream Analytics para chamar o serviço Web implantado

1. Em **Topologia de Trabalho**, clique em **Funções** > **Adicionar**.
1. Insira as seguintes informações:

   **Alias da função**: Digite `machinelearning`.

   **Tipo de função**: Selecione **Azure ML**.

   **Opção de importação**: **Selecione Importar de uma assinatura diferente**.

   **URL**: Insira a URL DO SERVIÇO WEB que você anotou da pasta de trabalho do Excel.

   **Chave**: Insira a TECLA DE ACESSO anotada da pasta de trabalho do Excel.

   ![Adicionar uma função ao trabalho do Stream Analytics no Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Clique em **Criar**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Configurar a consulta do trabalho do Stream Analytics

1. Em **Topologia do Trabalho**, clique em **Consulta**.
1. Substitua o código existente pelo seguinte código:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[scored probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Substitua `[YourInputAlias]` pelo alias de entrada do trabalho.

   Substitua `[YourOutputAlias]` pelo alias de saída do trabalho.

1. Clique em **Save** (Salvar).

### <a name="run-the-stream-analytics-job"></a>Executar o trabalho do Stream Analytics

No trabalho do Stream Analytics, clique em **Iniciar** > **Agora** > **Iniciar**. Depois que o trabalho é iniciado com êxito, o status do trabalho muda de **parado** para **executando**.

![Executar o trabalho do Stream Analytics](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Usar o Gerenciador de Armazenamento do Microsoft Azure para exibir a previsão do tempo

Execute o aplicativo cliente para iniciar a coleta e o envio de dados de temperatura e umidade para o Hub IoT. Para cada mensagem recebida pelo Hub IoT, o trabalho do Stream Analytics chama o serviço Web de previsão do tempo para produzir a possibilidade de chuva. O resultado é então salvo no armazenamento de blobs do Azure. O Gerenciador de Armazenamento do Azure é uma ferramenta que pode ser usada para exibir o resultado.

1. [Baixe e instale o Gerenciador de Armazenamento do Microsoft Azure](https://storageexplorer.com/).
1. Abra o Gerenciador de Armazenamento do Azure.
1. Entre em sua conta do Azure.
1. Selecione sua assinatura.
1. Clique em sua assinatura > **Contas de Armazenamento** > sua conta de armazenamento > **Contêineres de Blobs** > seu contêiner.
1. Fala o download de um arquivo .csv para ver o resultado. A última coluna registra a possibilidade de chuva.

   ![Obter resultados da previsão do tempo com o Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/weather-forecast-result.png)

## <a name="summary"></a>Resumo

Você usou bem o Azure Machine Learning para produzir a possibilidade de chuva com base nos dados de temperatura e umidade recebidos pelo Hub IoT.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
