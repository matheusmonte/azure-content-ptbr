<properties
   pageTitle="Usando o Conector de Recursos do Azure nos Aplicativos lógicos | Serviço de Aplicativo do Microsoft Azure"
   description="Como criar e configurar o Conector de Recursos do Azure ou o aplicativo de API e usá-lo em um Aplicativo lógico no Serviço de Aplicativo do Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# Introdução ao Conector de Recursos do Azure e adição dele ao seu Aplicativo lógico
>[AZURE.NOTE] Esta versão do artigo aplica-se à versão do esquema 2014-12-01-preview dos Aplicativos Lógicos.

Use o Conector de Recursos do Azure para gerenciar facilmente os Recursos do Azure em seu Aplicativo lógico.

## Criar o conector de recursos do Azure
Para usar o aplicativo de API do Conector de Recursos do Azure, primeiro você precisa criar uma instância dele. Isso pode ser feito de forma embutida durante a criação de um aplicativo lógico ou selecionando o aplicativo de API do Conector do Gerenciador de recursos do Azure no Azure Marketplace.

Para configurá-lo, você precisa definir a uma entidade de serviço com permissões para fazer tudo o que você deseja fazer no Azure. Todas as chamadas serão feitas em nome dessa entidade de serviço que você configurou. Isso permite que você defina o escopo do conector para usar somente exatamente o que você deseja que ele faça e nada mais.

David Ebbo escreveu [uma postagem de blog excelente](http://blog.davidebbo.com/2014/12/azure-service-principal.html) sobre como fazer essa configuração. Siga todas as instruções e obtenha sua **ID do locatário**, **ID do cliente** e **Senha**. Esses três campos, mais a **ID da assinatura**, são o que é necessário para configurar o conector.

## Usando o Conector de Recursos do Azure no designer de Aplicativos lógicos
### Gatilho
Há dois gatilhos que têm suporte no Conector:

Nome | Descrição
---- | -----------
Evento ocorre | Gatilho quando ocorre um evento para um recurso em sua assinatura.
A métrica cruza o limite | Gatilho quando uma métrica atinge um determinado limite.

### Ação

Da mesma forma, você pode fornecer um grande número de ações dentro de sua assinatura do Azure:

Para **grupos de recursos**, você pode:

Nome | Descrição
---- | -----------
Listar os grupos de recursos | Listar todos os grupos de recursos na assinatura.
Obter o grupo de recursos | Obter um grupo de recursos de acordo com sua ID.
Criar grupo de recursos | Escolher ou atualizar um grupo de recursos.
Excluir grupo de recursos | Excluir um grupo de recursos.

Para **Recursos**, você pode:

Nome | Descrição
---- | -----------
Listar recursos | Listar os recursos em sua assinatura de acordo com os diferentes tipos de filtros.
Obter recurso | Obter um único recurso de acordo com sua ID de recurso.
Criar ou atualizar recurso | Criar um recurso ou atualizar um recurso existente. Você deve fornecer todas as propriedades para esse recurso.
Ação de recursos | Execute qualquer outra ação em um recurso. Você precisa saber o nome da ação e a carga que essa ação precisa (se houver).
Excluir recurso | Excluir um recurso.

Para **provedores de recursos**, você pode:

Nome | Descrição
---- | -----------
Listar provedores de recursos | Listar todos os provedores de recursos disponíveis na assinatura.

Para **Implantações de Grupos de Recursos**, você pode:

Nome | Descrição
---- | -----------
Listar implantações | Listar todas as implantações em um grupo de recursos.
Obter implantação | Obter uma implantação de modelo de acordo com sua ID.
Criar implantação | Criar uma nova implantação de grupo de recursos fornecendo um modelo.

Para **eventos** sobre os recursos, você pode:

Nome | Descrição
---- | -----------
Obter eventos | Obter eventos em uma assinatura ou para um recurso.

Para **métricas** sobre os recursos, você pode:

Nome | Descrição
---- | -----------
Obter métricas | Obter uma métrica para uma ID de recurso.

## Fazer mais com seu conector
Agora que o conector foi criado, você pode adicioná-lo a um fluxo comercial usando um Aplicativo Lógico. Consulte [O que são Aplicativos lógicos?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Se você quiser começar com os Aplicativos lógicos do Azure antes de se inscrever em uma conta do Azure, acesse [Experimentar Aplicativos lógicos](https://tryappservice.azure.com/?appservice=logic), onde poderá criar imediatamente um Aplicativo lógico inicial de curta duração no Serviço de Aplicativo. Nenhum cartão de crédito é exigido, sem compromissos.

Exiba a referência da API REST do Swagger em [Conectores e Referência dos aplicativos de API](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md

<!---HONumber=AcomDC_0907_2016-->