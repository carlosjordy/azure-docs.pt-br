---
title: Referência de integração do Azure Active Directory e do SAP SuccessFactors
description: Aprofundamento técnico no provisionamento direcionado do SAP SuccessFactors-HR
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: reference
ms.workload: identity
ms.date: 07/20/2020
ms.author: chmutali
ms.openlocfilehash: 3c1d0d05554fafb4b18d8dc7043cca3e8479b35e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87095761"
---
# <a name="how-azure-active-directory-provisioning-integrates-with-sap-successfactors"></a>Como Azure Active Directory provisionamento se integra ao SAP SuccessFactors 

[Azure Active Directory serviço de provisionamento de usuário](../app-provisioning/user-provisioning.md) integra-se ao [SAP SuccessFactors Employee central](https://www.successfactors.com/products-services/core-hr-payroll/employee-central.html) para gerenciar o ciclo de vida de identidade dos usuários. O Azure Active Directory oferece três integrações predefinidas: 

* SuccessFactors no local Active Directory provisionamento de usuário
* SuccessFactors para Azure Active Directory provisionamento de usuário
* Write-back SuccessFactors

Este artigo explica como a integração funciona e como você pode personalizar o comportamento de provisionamento para diferentes cenários de RH. 

## <a name="establishing-connectivity"></a>Estabelecendo conectividade 
O mecanismo de provisionamento do Azure AD usa a autenticação básica para se conectar aos pontos de extremidade da API do OData central. Ao configurar o aplicativo de provisionamento SuccessFactors, use o parâmetro *URL do locatário* na seção *credenciais de administrador* para configurar a [API Data Center URL](https://apps.support.sap.com/sap/support/knowledge/en/2215682). 

Para proteger ainda mais a conectividade entre o serviço de provisionamento do Azure AD e o SuccessFactors, você pode adicionar os intervalos de IP do Azure AD na lista de permissões de IP do SuccessFactors usando as etapas descritas abaixo:

* Baixar os [intervalos de IP mais recentes](https://www.microsoft.com/download/details.aspx?id=56519) para a nuvem pública do Azure 
* Abra o arquivo e procure as marcas **AzureActiveDirectory** e **AzureActiveDirectoryDomainServices** 

  >[!div class="mx-imgBorder"] 
  >![Intervalo de IPS do Azure AD](media/sap-successfactors-integration-reference/azure-active-directory-ip-range.png)

* Copie todos os intervalos de endereços IP listados no elemento *addressPrefixes* e use o intervalo para criar sua lista de restrições de endereço IP.
* Traduza os valores de CIDR em intervalos de IP.  
* Faça logon no portal de administração do SuccessFactors para adicionar intervalos de IP à lista de permissões. Consulte a [Nota de suporte SAP 2253200](https://apps.support.sap.com/sap/support/knowledge/en/2253200). Agora você pode [Inserir intervalos de IP](https://answers.sap.com/questions/12882263/whitelisting-sap-cloud-platform-ip-address-range-i.html) nesta ferramenta. 

## <a name="supported-entities"></a>Entidades com suporte
Para cada usuário no SuccessFactors, o serviço de provisionamento do Azure AD recupera as entidades a seguir. Cada entidade é expandida usando a API OData *$Expand* parâmetro de consulta. Consulte a coluna de *regra de recuperação* abaixo. Algumas entidades são expandidas por padrão, enquanto algumas entidades são expandidas somente se um atributo específico estiver presente no mapeamento. 

| \# | Entidade SuccessFactors                  | Nó do OData     | Regra de recuperação |
|----|----------------------------------------|------------------------------|------------------|
| 1  | Perpessoa                              | *nó raiz*                  | Sempre           |
| 2  | Entre pessoas                            | personalInfoNav              | Sempre           |
| 3  | Por telefone                               | phoneNav                     | Sempre           |
| 4  | Enviar por email                               | emailNav                     | Sempre           |
| 5  | EmpEmployment                          | employmentNav                | Sempre           |
| 6  | Usuário                                   | employmentNav/userNav        | Sempre           |
| 7  | EmpJob                                 | employmentNav/jobInfoNav     | Sempre           |
| 8  | EmpEmploymentTermination               | activeEmploymentsCount       | Sempre           |
| 9  | FOCompany                              | employmentNav/jobInfoNav/companyNav | Somente se o atributo Company ou CompanyID for mapeado |
| 10 | FODepartment                           | employmentNav/jobInfoNav/departmentNav | Somente se o atributo Department ou DepartmentID for mapeado |
| 11 | FOBusinessUnit                         | employmentNav/jobInfoNav/businessUnitNav | Somente se o atributo businessUnit ou businessUnitId for mapeado |
| 12 | FOCostCenter                           | employmentNav/jobInfoNav/costCenterNav | Somente se o atributo costCenter ou costCenterId for mapeado |
| 13 | FODivision                             | employmentNav/jobInfoNav/divisionNav  | Somente se o atributo Division ou divisionid for mapeado |
| 14 | FOJobCode                              | employmentNav/jobInfoNav/jobCodeNav  | Somente se o atributo jobCode ou jobCodeId for mapeado |
| 15 | FOPayGrade                             | employmentNav/jobInfoNav/payGradeNav  | Somente se o atributo payGrade for mapeado |
| 16 | FOLocation                             | employmentNav/jobInfoNav/locationNav  | Somente se o atributo Location for mapeado |
| 17 | FOCorporateAddressDEFLT                | employmentNav/jobInfoNav/addressNavDEFLT  | Se o mapeamento contiver um dos seguintes atributos: officeLocationAddress, officeLocationCity, officeLocationZipCode |
| 18 | FOEventReason                          | employmentNav/jobInfoNav/eventReasonNav  | Somente se o atributo eventReason for mapeado |
| 19 | EmpGlobalAssignment                    | employmentNav/empGlobalAssignmentNav | Somente se assignmenttype for mapeado |
| 20 | Lista de seleção de empregos                | employmentNav/jobInfoNav/employmentTypeNav | Somente se o contratador for mapeado |
| 21 | Lista de seleção de EmployeeClass                 | employmentNav/jobInfoNav/employeeClassNav | Somente se employeeClass for mapeado |
| 22 | Lista de seleção de EmplStatus                    | employmentNav/jobInfoNav/emplStatusNav | Somente se emplStatus for mapeado |
| 23 | Lista de seleção de assignmenttype                | employmentNav/empGlobalAssignmentNav/assignmentTypeNav | Somente se assignmenttype for mapeado |

## <a name="how-full-sync-works"></a>Como funciona a sincronização completa
Com base no mapeamento de atributo, durante a sincronização completa, o serviço de provisionamento do Azure AD envia a seguinte consulta de API OData "GET" para buscar dados efetivos de todos os usuários ativos. 

> [!div class="mx-tdCol2BreakAll"]
>| Parâmetro | Descrição |
>| ----------|-------------|
>| Host de API OData | Anexa https à URL do *locatário*. Exemplo: `https://api4.successfactors.com` |
>| Ponto de extremidade da API OData | `/odata/v2/PerPerson` |
>| Parâmetro de consulta de $format OData | `json` |
>| Parâmetro de consulta de $filter OData | `(personEmpTerminationInfoNav/activeEmploymentsCount ge 1) and (lastModifiedDateTime le <CurrentExecutionTime>)` |
>| Parâmetro de consulta de $expand OData | Esse valor de parâmetro depende dos atributos mapeados. Exemplo: `employmentNav/userNav,employmentNav/jobInfoNav,personalInfoNav,personEmpTerminationInfoNav,phoneNav,emailNav,employmentNav/jobInfoNav/companyNav/countryOfRegistrationNav,employmentNav/jobInfoNav/divisionNav,employmentNav/jobInfoNav/departmentNav` |
>| Parâmetro de consulta OData customPageSize | `100` |

> [!NOTE]
> Durante a primeira sincronização completa inicial, o serviço de provisionamento do Azure AD não obtém dados de trabalho inativos/encerrados.

Para cada usuário do SuccessFactors, o serviço de provisionamento procura uma conta no destino (Azure AD/local Active Directory) usando o atributo correspondente definido no mapeamento. Por exemplo: se *personIdExternal* for mapeado para *EmployeeID* e for definido como o atributo correspondente, o serviço de provisionamento usará o valor *personIdExternal* para pesquisar o usuário com o filtro *EmployeeID* . Se uma correspondência de usuário for encontrada, ela atualizará os atributos de destino. Se nenhuma correspondência for encontrada, ela criará uma nova entrada no destino. 

Para validar os dados retornados por seu ponto de extremidade de API OData para um *personIdExternal*específico, atualize o *SuccessFactorsAPIEndpoint* na consulta de API abaixo com sua API Data Center servidor URL e use uma ferramenta como o [postmaster](https://www.postman.com/downloads/) para invocar a consulta. 

```
https://[SuccessFactorsAPIEndpoint]/odata/v2/PerPerson?$format=json&
$filter=(personIdExternal in '[personIdExternalValue]')&
$expand=employmentNav/userNav,employmentNav/jobInfoNav,personalInfoNav,personEmpTerminationInfoNav,
phoneNav,phoneNav/phoneTypeNav,emailNav,employmentNav/jobInfoNav/businessUnitNav,employmentNav/jobInfoNav/companyNav,
employmentNav/jobInfoNav/companyNav/countryOfRegistrationNav,employmentNav/jobInfoNav/costCenterNav,
employmentNav/jobInfoNav/divisionNav,employmentNav/jobInfoNav/departmentNav,employmentNav/jobInfoNav/jobCodeNav,
employmentNav/jobInfoNav/locationNav,employmentNav/jobInfoNav/locationNav/addressNavDEFLT,employmentNav/jobInfoNav/payGradeNav,
employmentNav/empGlobalAssignmentNav,employmentNav/empGlobalAssignmentNav/assignmentTypeNav,employmentNav/jobInfoNav/emplStatusNav,
employmentNav/jobInfoNav/employmentTypeNav,employmentNav/jobInfoNav/employeeClassNav,employmentNav/jobInfoNav/eventReasonNav
```

## <a name="how-incremental-sync-works"></a>Como funciona a sincronização incremental

Após a sincronização completa, o serviço de provisionamento do Azure AD mantém o *LastExecutionTimestamp* e o utiliza para criar consultas Delta para recuperar alterações incrementais. Os atributos de carimbo de data/hora presentes em cada entidade SuccessFactors, como *lastModifiedDateTime*, *StartDate*, *EndDate*e *latestTerminationDate*, são avaliados para ver se a alteração cai entre *LastExecutionTimestamp* e *CurrentExecutionTime*. Em caso afirmativo, a alteração de entrada é considerada efetiva e processada para sincronização. 

## <a name="reading-attribute-data"></a>Lendo dados de atributo

Quando o serviço de provisionamento do Azure AD consulta SuccessFactors, ele recupera um conjunto de resultados JSON. O conjunto de resultados JSON inclui um número de atributos armazenados no funcionário central. Por padrão, o esquema de provisionamento é configurado para recuperar apenas um subconjunto desses atributos. 

Para recuperar atributos adicionais, siga as etapas listadas abaixo:
    
* Navegue até **aplicativos empresariais**  ->  **SuccessFactors**  ->  **provisionamento**de aplicativo  ->  **Editar**  ->  **página mapeamento de atributo**de provisionamento.
* Role para baixo e clique em **Mostrar opções avançadas**.
* Clique em **Editar lista de atributos para SuccessFactors**. 

> [!NOTE] 
> Se a opção **Editar lista de atributos para SuccessFactors** não aparecer no portal do Azure, use a URL *https://portal.azure.com/?Microsoft_AAD_IAM_forceSchemaEditorEnabled=true* para acessar a página. 

* A coluna **expressão de API** nessa exibição exibe as expressões JSONPath usadas pelo conector.
  >[!div class="mx-imgBorder"] 
  >![Expressão de API](media/sap-successfactors-integration-reference/jsonpath-api-expressions.png#lightbox)  
* Você pode editar um valor JSONPath existente ou adicionar um novo atributo com uma expressão JSONPath válida ao esquema. 

A próxima seção fornece uma lista de cenários comuns para editar os valores de JSONPath. 

## <a name="handling-different-hr-scenarios"></a>Lidando com cenários de RH diferentes

JSONPath é uma linguagem de consulta para JSON semelhante ao XPath for XML. Como o XPath, o JSONPath permite a extração e a filtragem de dados de uma carga JSON.
Usando a transformação JSONPath, você pode personalizar o comportamento do aplicativo de provisionamento do Azure AD para recuperar atributos personalizados e manipular cenários como recontratar, conversão de trabalho e atribuição global. 

Esta seção aborda como você pode personalizar o aplicativo de provisionamento para os seguintes cenários de RH: 
* [Recuperando atributos adicionais](#retrieving-additional-attributes)
* [Recuperando atributos personalizados](#retrieving-custom-attributes)
* [Manipulando cenário de conversão de trabalho](#handling-worker-conversion-scenario)
* [Manipulando o cenário de recontratação](#handling-rehire-scenario)
* [Tratando do cenário de atribuição global](#handling-global-assignment-scenario)
* [Manipulando o cenário de trabalhos simultâneos](#handling-concurrent-jobs-scenario)

### <a name="retrieving-additional-attributes"></a>Recuperando atributos adicionais

O esquema padrão do aplicativo de provisionamento SuccessFactors do Azure AD é fornecido com mais de [90 atributos predefinidos](sap-successfactors-attribute-reference.md). Para adicionar mais atributos SuccessFactors prontos para o esquema de provisionamento, use as etapas listadas abaixo: 

* Use a consulta OData abaixo para recuperar dados de um usuário de teste válido da central de funcionários. 

```
    https://[SuccessFactorsAPIEndpoint]/odata/v2/PerPerson?$format=json&
    $filter=(personIdExternal in '[personIdExternalValue]')&
    $expand=employmentNav/userNav,employmentNav/jobInfoNav,personalInfoNav,personEmpTerminationInfoNav,
    phoneNav,phoneNav/phoneTypeNav,emailNav,employmentNav/jobInfoNav/businessUnitNav,employmentNav/jobInfoNav/companyNav,
    employmentNav/jobInfoNav/companyNav/countryOfRegistrationNav,employmentNav/jobInfoNav/costCenterNav,
    employmentNav/jobInfoNav/divisionNav,employmentNav/jobInfoNav/departmentNav,employmentNav/jobInfoNav/jobCodeNav,
    employmentNav/jobInfoNav/locationNav,employmentNav/jobInfoNav/locationNav/addressNavDEFLT,employmentNav/jobInfoNav/payGradeNav,
    employmentNav/empGlobalAssignmentNav,employmentNav/empGlobalAssignmentNav/assignmentTypeNav,employmentNav/jobInfoNav/emplStatusNav,
    employmentNav/jobInfoNav/employmentTypeNav,employmentNav/jobInfoNav/employeeClassNav,employmentNav/jobInfoNav/eventReasonNav
```

* Determinar a entidade do funcionário central associada ao atributo
  * Se o atributo fizer parte da entidade *EmpEmployment* , procure o atributo em nó *employmentNav* . 
  * Se o atributo fizer parte da entidade do *usuário* , procure o atributo em nó *employmentNav/userNav* .
  * Se o atributo fizer parte da entidade *EmpJob* , procure o atributo no nó *employmentNav/jobInfoNav* . 
* Construa o caminho JSON associado ao atributo e adicione esse novo atributo à lista de atributos SuccessFactors. 
  * Exemplo 1: digamos que você queira adicionar o atributo *okToRehire*, que faz parte da entidade *employmentNav* , em seguida, use o JSONPath`$.employmentNav.results[0].okToRehire`
  * Exemplo 2: digamos que você queira adicionar o *fuso horário*do atributo, que faz parte da entidade *userNav* , em seguida, use o JSONPath`$.employmentNav.results[0].userNav.timeZone`
  * Exemplo 3: digamos que você queira adicionar o atributo *flsaStatus*, que faz parte da entidade *jobInfoNav* , em seguida, use o JSONPath`$.employmentNav.results[0].jobInfoNav.results[0].flsaStatus`
* Salve o esquema. 
* Reinicie o provisionamento.

### <a name="retrieving-custom-attributes"></a>Recuperando atributos personalizados

Por padrão, os seguintes atributos personalizados são predefinidos no aplicativo de provisionamento SuccessFactors do Azure AD: 
* *custom01-custom15* da entidade User (userNav)
* *customString1-customString15* da entidade EmpEmployment (employmentNav) chamada *empNavCustomString1-empNavCustomString15*
* *customString1-customString15* da entidade EmpJobInfo (jobInfoNav) chamada *empJobNavCustomString1-empNavJobCustomString15*

Digamos, em sua instância central de funcionários, o atributo *customString35* em *EmpJobInfo* armazena a descrição do local. Você deseja fluir esse valor para Active Directory atributo *physicalDeliveryOfficeName* . Para configurar o mapeamento de atributos para esse cenário, use as etapas fornecidas abaixo: 

* Edite a lista de atributos SuccessFactors para adicionar um novo atributo chamado *empJobNavCustomString35*.
* Defina a expressão da API JSONPath para este atributo como:`$.employmentNav.results[0].jobInfoNav.results[0].customString35`
* Salve e recarregue a alteração de mapeamento no portal do Azure.  
* Na folha mapeamento de atributo, mapeie *empJobNavCustomString35* para *physicalDeliveryOfficeName*.
* Salve o mapeamento.

Estendendo este cenário: 
* Se você quiser mapear o atributo *custom35* da entidade de *usuário* , use o JSONPath`$.employmentNav.results[0].userNav.custom35`
* Se você quiser mapear o atributo *customString35* da entidade *EmpEmployment* , use o JSONPath`$.employmentNav.results[0].customString35`

### <a name="handling-worker-conversion-scenario"></a>Manipulando cenário de conversão de trabalho

A conversão de trabalho é o processo de converter um funcionário em tempo integral existente para um prestador de atendimento ou vice-versa. Nesse cenário, a central de funcionários adiciona uma nova entidade *EmpEmployment* junto com uma nova entidade de *usuário* para a mesma entidade *Person* . A entidade de *usuário* aninhada na entidade *EmpEmployment* anterior está definida como NULL. Para lidar com esse cenário para que os novos dados de emprego apareçam quando ocorre uma conversão, você pode atualizar em massa o esquema do aplicativo de provisionamento usando as etapas listadas abaixo:  

* Abra a folha mapeamento de atributos do seu aplicativo de provisionamento SuccessFactors. 
* Role para baixo e clique em **Mostrar opções avançadas**.
* Clique no link **revisar seu esquema aqui** para abrir o editor de esquema. 
  >![análise-esquema](media/sap-successfactors-integration-reference/review-schema.png#lightbox)
* Clique no link **baixar** para salvar uma cópia do esquema antes de editar. 
  >![baixar-esquema](media/sap-successfactors-integration-reference/download-schema.png#lightbox)
* No editor de esquema, pressione a tecla Ctrl-H para abrir o controle localizar-substituir.
* Na caixa de texto Localizar, copie e cole o valor`$.employmentNav.results[0]`
* Na caixa de texto substituir, copie e cole o valor `$.employmentNav.results[?(@.userNav != null)]` . Observe o espaço em branco ao redor do `!=` operador, que é importante para o processamento bem-sucedido da expressão JSONPath. 
  >![localizar-substituir-conversão](media/sap-successfactors-integration-reference/find-replace-conversion-scenario.png#lightbox)
* Clique na opção "substituir tudo" para atualizar o esquema. 
* Salve o esquema. 
* O processo acima atualiza todas as expressões JSONPath da seguinte maneira: 
  * JSONPath antigo:`$.employmentNav.results[0].jobInfoNav.results[0].departmentNav.name_localized`
  * Novo JSONPath:`$.employmentNav.results[?(@.userNav != null)].jobInfoNav.results[0].departmentNav.name_localized`
* Reinicie o provisionamento. 

### <a name="handling-rehire-scenario"></a>Manipulando o cenário de recontratação

Geralmente, há duas opções para processar recontratações: 
* Opção 1: criar um novo perfil de pessoa na central do funcionário
* Opção 2: reutilizar o perfil de pessoa existente na central do funcionário

Se o processo de RH usar a opção 1, nenhuma alteração será necessária para o esquema de provisionamento. Se o processo de RH usar a opção 2, a central de funcionários adicionará uma nova entidade *EmpEmployment* junto com uma nova entidade de *usuário* para a mesma entidade *Person* . Ao contrário do cenário de conversão, a entidade *EmpEmployment* anterior retém a entidade de *usuário* e não é definida como NULL. 

Para lidar com esse cenário de recontratação (opção 2), para que os dados de emprego mais recentes sejam exibidos para perfis de recontratação, você pode atualizar em massa o esquema do aplicativo de provisionamento usando as etapas listadas abaixo:  

* Abra a folha mapeamento de atributos do seu aplicativo de provisionamento SuccessFactors. 
* Role para baixo e clique em **Mostrar opções avançadas**.
* Clique no link **revisar seu esquema aqui** para abrir o editor de esquema.   
* Clique no link **baixar** para salvar uma cópia do esquema antes de editar.   
* No editor de esquema, pressione a tecla Ctrl-H para abrir o controle localizar-substituir.
* Na caixa de texto Localizar, copie e cole o valor`$.employmentNav.results[0]`
* Na caixa de texto substituir, copie e cole o valor `$.employmentNav.results[-1:]` . Essa expressão JSONPath retorna o registro de *EmpEmployment* mais recente.   
* Clique na opção "substituir tudo" para atualizar o esquema. 
* Salve o esquema. 
* O processo acima atualiza todas as expressões JSONPath da seguinte maneira: 
  * JSONPath antigo:`$.employmentNav.results[0].jobInfoNav.results[0].departmentNav.name_localized`
  * Novo JSONPath:`$.employmentNav.results[-1:].jobInfoNav.results[0].departmentNav.name_localized`
* Reinicie o provisionamento. 

### <a name="handling-global-assignment-scenario"></a>Tratando do cenário de atribuição global

Quando um usuário na central do funcionário é processado para atribuição global, o SuccessFactors adiciona uma nova entidade *EmpEmployment* e define o *ASSIGNMENTCLASS* como "Ga". Ele também cria uma nova entidade de *usuário* . Portanto, o usuário agora tem:
* Uma *EmpEmployment*  +  entidade de*usuário* EmpEmployment que corresponde à atribuição inicial com *assignmentClass* definido como "St" e 
* Outra *EmpEmployment*  +  entidade de*usuário* EmpEmployment que corresponde à atribuição global com *assignmentClass* definido como "Ga"

Para buscar atributos que pertencem à atribuição padrão e ao perfil de usuário de atribuição global, use as etapas listadas abaixo: 

* Abra a folha mapeamento de atributos do seu aplicativo de provisionamento SuccessFactors. 
* Role para baixo e clique em **Mostrar opções avançadas**.
* Clique no link **revisar seu esquema aqui** para abrir o editor de esquema.   
* Clique no link **baixar** para salvar uma cópia do esquema antes de editar.   
* No editor de esquema, pressione a tecla Ctrl-H para abrir o controle localizar-substituir.
* Na caixa de texto Localizar, copie e cole o valor`$.employmentNav.results[0]`
* Na caixa de texto substituir, copie e cole o valor `$.employmentNav.results[?(@.assignmentClass == 'ST')]` . 
* Clique na opção "substituir tudo" para atualizar o esquema. 
* Salve o esquema. 
* O processo acima atualiza todas as expressões JSONPath da seguinte maneira: 
  * JSONPath antigo:`$.employmentNav.results[0].jobInfoNav.results[0].departmentNav.name_localized`
  * Novo JSONPath:`$.employmentNav.results[?(@.assignmentClass == 'ST')].jobInfoNav.results[0].departmentNav.name_localized`
* Recarregue a folha de mapeamento de atributo do aplicativo. 
* Role para baixo e clique em **Mostrar opções avançadas**.
* Clique em **Editar lista de atributos para SuccessFactors**.
* Adicione novos atributos para buscar dados de atribuição global. Por exemplo: se você quiser buscar o nome do departamento associado a um perfil de atribuição global, poderá adicionar o atributo **globalAssignmentDepartment** com a expressão JSONPath definida como `$.employmentNav.results[?(@.assignmentClass == 'GA')].jobInfoNav.results[0].departmentNav.name_localized` . 
* Agora você pode fluir ambos os valores de departamento para Active Directory atributos ou fluir seletivamente um valor usando o mapeamento de expressão. Exemplo: a expressão abaixo define o valor do atributo *Departamento* do AD como *globalAssignmentDepartment* se ele estiver presente, caso contrário, ele definirá o valor para *Departamento* associado à atribuição padrão. 
  * `IIF(IsPresent([globalAssignmentDepartment]),[globalAssignmentDepartment],[department])`
* Salve o mapeamento. 
* Reinicie o provisionamento. 

### <a name="handling-concurrent-jobs-scenario"></a>Manipulando o cenário de trabalhos simultâneos

Quando um usuário na central do funcionário tem trabalhos simultâneos/múltiplos, há duas entidades *EmpEmployment* e *usuário* com *assignmentClass* definido como "St". Para buscar atributos que pertencem a ambos os trabalhos, use as etapas listadas abaixo: 

* Abra a folha mapeamento de atributos do seu aplicativo de provisionamento SuccessFactors. 
* Role para baixo e clique em **Mostrar opções avançadas**.
* Clique em **Editar lista de atributos para SuccessFactors**.
* Digamos que você deseja efetuar pull do departamento associado ao trabalho 1 e ao trabalho 2. O *Departamento* de atributos predefinido já busca o valor do departamento para o primeiro trabalho. Você pode definir um novo atributo chamado *secondJobDepartment* e definir a expressão JSONPath como`$.employmentNav.results[1].jobInfoNav.results[0].departmentNav.name_localized`
* Agora você pode fluir ambos os valores de departamento para Active Directory atributos ou fluir seletivamente um valor usando o mapeamento de expressão. 
* Salve o mapeamento. 
* Reinicie o provisionamento. 

## <a name="next-steps"></a>Próximas etapas

* [Saiba como configurar o SuccessFactors para Active Directory provisionamento](../saas-apps/sap-successfactors-inbound-provisioning-tutorial.md)
* [Saiba como configurar o Write-back para SuccessFactors](../saas-apps/sap-successfactors-writeback-tutorial.md)
* [Saiba mais sobre os atributos SuccessFactors com suporte para o provisionamento de entrada](sap-successfactors-attribute-reference.md)










