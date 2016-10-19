<properties
    pageTitle="Adicionar o conector do Informix ao seus Aplicativos Lógicos | Microsoft Azure"
    description="Visão geral do Conector do Informix com os parâmetros da API REST"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# Introdução ao conector do Informix
O Microsoft Connector para Informix conecta os Aplicativos Lógicos aos recursos armazenados em um banco de dados IBM Informix. O conector Informix inclui um cliente Microsoft para se comunicar com computadores de servidores Informix remotos em uma rede TCP/IP. Isso inclui bancos de dados de nuvem, como o IBM Informix para Windows em execução na virtualização do Azure e bancos de dados locais usando o gateway de dados local. Veja a [lista](connectors-create-api-informix.md#supported-informix-platforms-and-versions) de plataformas e versões do IBM Informix com suporte (neste tópico).

>[AZURE.NOTE] Esta versão do artigo se aplica à disponibilidade de Aplicativos Lógicos em geral (GA).

O conector oferece suporte às seguintes operações de banco de dados:

- Listar tabelas do banco de dados
- Ler uma linha usando SELECT
- Ler todas as linhas usando SELECT
- Adicionar uma linha usando INSERT
- Alterar uma linha usando UPDATE
- Remover uma linha usando DELETE

Este tópico mostra como usar o Conector em um aplicativo lógico para processar as operações do banco de dados.

Para saber mais sobre os Aplicativos Lógicos, consulte como [criar um aplicativo lógico](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Ações disponíveis
Este conector dá suporte às seguintes ações do aplicativo lógico:

- Getables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## Listar tabelas
A criação de um aplicativo lógico para qualquer operação é composta de várias etapas executadas por meio do portal do Microsoft Azure.

No aplicativo lógico, você pode adicionar uma ação para listar tabelas em um banco de dados Informix. A ação instrui o conector a processar uma instrução de esquema do Informix, como `CALL SYSIBM.SQLTABLES`.

### Criar um aplicativo lógico
1.	No **painel inicial do Azure**, selecione **+** (sinal de mais), **Web + Móvel**, então, **Aplicativo Lógico**.
2.	Insira o **Nome**, como `InformixgetTables`, **Assinatura**, **Grupo de recursos**, **Local** e **Plano do Serviço de Aplicativo**. Escolha **Fixar no painel** e selecione **Criar**.

### Adicionar um gatilho e uma ação
1.	No **Designer de Aplicativos Lógicos**, selecione **Aplicativo Lógico em Branco** na lista **Modelos**.
2.	Na lista de **gatilhos**, selecione **Recorrência**.
3.	No gatilho **Recorrência**, clique em **Editar**, escolha a lista suspensa **Frequência** para selecionar **Dia** e selecione **Intervalo** para digitar **7**.
4.	Selecione a caixa **+ Nova etapa** caixa e escolha **Adicionar uma ação**.
5.	Na lista de **ações**, digite **informix** na caixa de edição **Procurar mais ações** e selecione **Informix - Obter tabelas (Visualização)**.

	![](./media/connectors-create-api-informix/InformixconnectorActions.png)

6.	No painel de configuração **Informix - Obter tabelas**, marque **caixa de seleção** para habilitar **Conectar via gateway de dados local**. Observe que as configurações mudam de nuvem para local.
	- Digite o valor para **Servidor**, na forma de número da porta com dois pontos para o endereço ou alias. Por exemplo, digite `ibmserver01:9089`.
	- Digite o valor para o **Banco de Dados**. Por exemplo, digite `nwind`.
	- Selecione o valor para a **Autenticação**. Por exemplo, selecione **Básica**.
	- Digite o valor para o **Nome de Usuário**. Por exemplo, digite `informix`.
	- Digite o valor para a **Senha**. Por exemplo, digite `Password1`.
	- Selecione o valor para o **Gateway**. Por exemplo, selecione **datagateway01**.
7. Escolha **Criar** e, em seguida, **Salvar**.

	![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

8.	Na folha **InformixgetTables**, na lista **Todas as execuções** em **Resumo**, selecione o item listado primeiro (a execução mais recente).
9.	Na folha **Execução do aplicativo lógico**, escolha **Detalhes da Execução**. Na lista **Ação**, clique em **Get\_tables**. Confira o valor para o **Status**, que deve ser **Êxito**. Selecione o **link Entradas** para exibir as entradas. Selecione o **link Saídas** e exiba as saídas, que deverão incluir uma lista de tabelas.

	![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## Criar as conexões
Este conector dá suporte às conexões com os bancos de dados no local e na nuvem usando as propriedades de conexão a seguir.

Propriedade | Descrição
--- | ---
server | Obrigatório. Aceita um valor da cadeia de caracteres que representa um endereço TCP/IP ou alias no formato IPv4 ou IPv6, seguido de (delimitado por dois pontos) um número de porta TCP/IP. 
database | Obrigatório. Aceita um valor da cadeia de caracteres que representa um Nome do Banco de Dados Relacional (RDBNAM) DRDA. O Informix aceita uma cadeia de caracteres de 128 bytes (o banco de dados é conhecido como um nome de banco de dados IBM Informix (dbname)).
autenticação | Opcional. Aceita um valor de item de lista, Básica ou Windows (kerberos). 
Nome de Usuário | Obrigatório. Aceita um valor de cadeia de caracteres.
Senha | Obrigatório. Aceita um valor de cadeia de caracteres.
gateway | Obrigatório. Aceita um valor de item da lista, que representa o gateway de dados local definido para os Aplicativos Lógicos no grupo de armazenamento.  

## Criar a conexão de gateway local
Este conector pode acessar um banco de dados do Informix local por meio do gateway de dados local. Consulte os tópicos do gateway para obter mais informações.

1. No painel de configuração **Gateways**, marque a **caixa de seleção** para habilitar **Conectar via gateway**. Veja as configurações mudarem de nuvem para local.
2. Digite o valor para **Servidor**, na forma de número da porta com dois pontos para o endereço ou alias. Por exemplo, digite `ibmserver01:9089`.
3. Digite o valor para o **Banco de Dados**. Por exemplo, digite `nwind`.
4. Selecione o valor para a **Autenticação**. Por exemplo, selecione **Básica**.
5. Digite o valor para o **Nome de Usuário**. Por exemplo, digite `informix`.
6. Digite o valor para a **Senha**. Por exemplo, digite `Password1`.
7. Selecione o valor para o **Gateway**. Por exemplo, selecione **datagateway01**.
8. Selecione **Criar** para continuar.

	![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## Criar a conexão de nuvem
Este conector pode acessar uma nuvem de banco de dados Informix.

1. No painel de configuração **Gateways**, deixe a **caixa de seleção** desabilitada (não clicada) **Conectar via gateway**.
2. Digite o valor para o **Nome da conexão**. Por exemplo, digite `hisdemo2`.
3. Digite o valor para o **Nome do servidor Informix**, na forma de número da porta com dois pontos para o endereço ou alias. Por exemplo, digite `hisdemo2.cloudapp.net:9089`.
3. Digite o valor para **nome do banco de dados Informix**. Por exemplo, digite `nwind`.
4. Digite o valor para o **Nome de Usuário**. Por exemplo, digite `informix`.
5. Digite o valor para a **Senha**. Por exemplo, digite `Password1`.
6. Selecione **Criar** para continuar.

	![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## Buscar todas as linhas usando SELECT
Você pode criar uma ação de aplicativo lógico para buscar todas as linhas na tabela do Informix. Essa ação instrui o conector a processar uma instrução SELECT do Informix, como `SELECT * FROM AREA`.

### Criar um aplicativo lógico
1.	No **painel inicial do Azure**, selecione **+** (sinal de mais), **Web + Móvel**, então, **Aplicativo Lógico**.
2.	Insira o **Nome** (por exemplo, "**InformixgetRows**"), **Assinatura**, **Grupo de recursos**, **Local** e **Plano do Serviço de Aplicativo**. Escolha **Fixar no painel** e selecione **Criar**.

### Adicionar um gatilho e uma ação
1.	No **Designer de Aplicativos Lógicos**, selecione **Aplicativo Lógico em Branco** na lista **Modelos**.
2.	Na lista de **gatilhos**, selecione **Recorrência**.
3.	No gatilho **Recorrência**, clique em **Editar**, escolha a lista suspensa **Frequência** para selecionar **Dia** e selecione **Intervalo** para digitar **7**.
4.	Selecione a caixa **+ Nova etapa** caixa e escolha **Adicionar uma ação**.
5.	Na lista de **ações**, digite **informix** na caixa de edição **Procurar mais ações** e selecione **Informix - Obter linhas (Visualização)**.
6. Na ação **Obter linhas (Visualização)**, selecione **Alterar conexão**.
7. No painel de configuração **Conexões**, selecione **Criar novo**.

	![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
  
8. No painel de configuração **Gateways**, deixe a **caixa de seleção** desabilitada (não clicada) **Conectar via gateway**.
	- Digite o valor para o **Nome da conexão**. Por exemplo, digite `HISDEMO2`.
	- Digite o valor para o **Nome do servidor Informix**, na forma de número da porta com dois pontos para o endereço ou alias. Por exemplo, digite `HISDEMO2.cloudapp.net:9089`.
	- Digite o valor para **nome do banco de dados Informix**. Por exemplo, digite `NWIND`.
	- Digite o valor para o **Nome de Usuário**. Por exemplo, digite `informix`.
	- Digite o valor para a **Senha**. Por exemplo, digite `Password1`.
9. Selecione **Criar** para continuar.

	![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

10. Na lista **Nome da tabela**, selecione a **seta para baixo** e selecione **AREA**.
11. Opcionalmente, selecione **Mostrar opções avançadas** para especificar as opções de consulta.
12. Selecione **Salvar**.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)

13.	Na folha **InformixgetRows**, na lista **Todas as execuções** em **Resumo**, selecione o item listado primeiro (a execução mais recente).
14.	Na folha **Execução do aplicativo lógico**, escolha **Detalhes da Execução**. Na lista **Ação**, selecione **Get\_rows**. Confira o valor para o **Status**, que deve ser **Êxito**. Selecione o **link Entradas** para exibir as entradas. Selecione o **link Saídas** e exiba as saídas, que deverão incluir uma lista de linhas.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## Adicionar uma linha usando INSERT
Você pode criar uma ação de aplicativo lógico para adicionar uma linha a uma tabela do Informix. Essa ação instrui o conector a processar uma instrução INSERT do Informix, como `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### Criar um aplicativo lógico
1.	No **painel inicial do Azure**, selecione **+** (sinal de mais), **Web + Móvel**, então, **Aplicativo Lógico**.
2.	Insira o **Nome**, como `InformixinsertRow`, **Assinatura**, **Grupo de recursos**, **Local** e **Plano do Serviço de Aplicativo**. Escolha **Fixar no painel** e selecione **Criar**.

### Adicionar um gatilho e uma ação
1.	No **Designer de Aplicativos Lógicos**, selecione **Aplicativo Lógico em Branco** na lista **Modelos**.
2.	Na lista de **gatilhos**, selecione **Recorrência**.
3.	No gatilho **Recorrência**, clique em **Editar**, escolha a lista suspensa **Frequência** para selecionar **Dia** e selecione **Intervalo** para digitar **7**.
4.	Selecione a caixa **+ Nova etapa** caixa e escolha **Adicionar uma ação**.
5.	Na lista de **ações**, digite **informix** na caixa de edição **Procurar mais ações** e selecione **Informix - Inserir linha (Visualização)**.
6. Na ação **Obter linhas (Visualização)**, selecione **Alterar conexão**.
7. No painel de configuração **Conexões**, selecione uma conexão. Por exemplo, selecione **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Na lista **Nome da tabela**, selecione a **seta para baixo** e selecione **AREA**.
9. Insira valores para todas as colunas necessárias (confira o asterisco vermelho). Por exemplo, digite `99999` para **AREAID**, digite `Area 99999` e digite `102` para **REGIONID**.
10. Selecione **Salvar**.

	![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
 
11.	Na folha **InformixinsertRow**, na lista **Todas as execuções** em **Resumo**, selecione o item listado primeiro (a execução mais recente).
12.	Na folha **Execução do aplicativo lógico**, escolha **Detalhes da Execução**. Na lista **Ação**, selecione **Get\_rows**. Confira o valor para o **Status**, que deve ser **Êxito**. Selecione o **link Entradas** para exibir as entradas. Selecione o **link Saídas** e exiba as saídas, que deverão incluir a nova linha.

	![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## Buscar uma linha usando SELECT
Você pode criar uma ação de aplicativo lógico para buscar uma linha em uma tabela do Informix. Essa ação instrui o conector para processar uma instrução SELECT WHERE do Informix, como `SELECT FROM AREA WHERE AREAID = '99999'`.

### Criar um aplicativo lógico
1.	No **painel inicial do Azure**, selecione **+** (sinal de mais), **Web + Móvel**, então, **Aplicativo Lógico**.
2.	Insira o **Nome**, como `InformixgetRow`, **Assinatura**, **Grupo de recursos**, **Local** e **Plano do Serviço de Aplicativo**. Escolha **Fixar no painel** e selecione **Criar**.

### Adicionar um gatilho e uma ação
1.	No **Designer de Aplicativos Lógicos**, selecione **Aplicativo Lógico em Branco** na lista **Modelos**.
2.	Na lista de **gatilhos**, selecione **Recorrência**.
3.	No gatilho **Recorrência**, clique em **Editar**, escolha a lista suspensa **Frequência** para selecionar **Dia** e selecione **Intervalo** para digitar **7**.
4.	Selecione a caixa **+ Nova etapa** caixa e escolha **Adicionar uma ação**.
5.	Na lista de **ações**, digite **informix** na caixa de edição **Procurar mais ações** e selecione **Informix - Obter linhas (Visualização)**.
6. Na ação **Obter linhas (Visualização)**, selecione **Alterar conexão**.
7. No painel de configurações de **Conexões**, selecione uma conexão existente. Por exemplo, selecione **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Na lista **Nome da tabela**, selecione a **seta para baixo** e selecione **AREA**.
9. Insira valores para todas as colunas necessárias (confira o asterisco vermelho). Por exemplo, digite `99999` para **AREAID**.
10. Opcionalmente, selecione **Mostrar opções avançadas** para especificar as opções de consulta.
11. Selecione **Salvar**.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)

12.	Na folha **InformixgetRow**, na lista **Todas as execuções** em **Resumo**, selecione o item listado primeiro (a execução mais recente).
13.	Na folha **Execução do aplicativo lógico**, escolha **Detalhes da Execução**. Na lista **Ação**, selecione **Get\_rows**. Confira o valor para o **Status**, que deve ser **Êxito**. Selecione o **link Entradas** para exibir as entradas. Selecione o **link Saídas** e exiba as saídas, que deverão incluir a linha.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## Alterar uma linha usando UPDATE
Você pode criar uma ação de aplicativo lógico para alterar uma linha em uma tabela do Informix. Essa ação instrui o conector a processar uma instrução UPDATE do Informix, como `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### Criar um aplicativo lógico
1.	No **painel inicial do Azure**, selecione **+** (sinal de mais), **Web + Móvel**, então, **Aplicativo Lógico**.
2.	Insira o **Nome**, como `InformixupdateRow`, **Assinatura**, **Grupo de recursos**, **Local** e **Plano do Serviço de Aplicativo**. Escolha **Fixar no painel** e selecione **Criar**.

### Adicionar um gatilho e uma ação
1.	No **Designer de Aplicativos Lógicos**, selecione **Aplicativo Lógico em Branco** na lista **Modelos**.
2.	Na lista de **gatilhos**, selecione **Recorrência**.
3.	No gatilho **Recorrência**, clique em **Editar**, escolha a lista suspensa **Frequência** para selecionar **Dia** e selecione **Intervalo** para digitar **7**.
4.	Selecione a caixa **+ Nova etapa** caixa e escolha **Adicionar uma ação**.
5.	Na lista de **ações**, digite **informix** na caixa de edição **Procurar mais ações** e selecione **Informix - Atualizar linha (Visualização)**.
6. Na ação **Obter linhas (Visualização)**, selecione **Alterar conexão**.
7. No painel de configurações de **Conexões**, selecione uma conexão existente. Por exemplo, selecione **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Na lista **Nome da tabela**, selecione a **seta para baixo** e selecione **AREA**.
9. Insira valores para todas as colunas necessárias (confira o asterisco vermelho). Por exemplo, digite `99999` para **AREAID**, digite `Updated 99999` e digite `102` para **REGIONID**.
10. Selecione **Salvar**.

	![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)

11.	Na folha **InformixupdateRow**, na lista **Todas as execuções** em **Resumo**, selecione o item listado primeiro (a execução mais recente).
12.	Na folha **Execução do aplicativo lógico**, escolha **Detalhes da Execução**. Na lista **Ação**, selecione **Get\_rows**. Confira o valor para o **Status**, que deve ser **Êxito**. Selecione o **link Entradas** para exibir as entradas. Selecione o **link Saídas** e exiba as saídas, que deverão incluir a nova linha.

	![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## Remover uma linha usando DELETE
Você pode criar uma ação de aplicativo lógico para remover uma linha de uma tabela do Informix. Essa ação instrui o conector a processar uma instrução DELETE do Informix, como `DELETE FROM AREA WHERE AREAID = '99999'`.

### Criar um aplicativo lógico
1.	No **painel inicial do Azure**, selecione **+** (sinal de mais), **Web + Móvel**, então, **Aplicativo Lógico**.
2.	Insira o **Nome**, como `InformixdeleteRow`, **Assinatura**, **Grupo de recursos**, **Local** e **Plano do Serviço de Aplicativo**. Escolha **Fixar no painel** e selecione **Criar**.

### Adicionar um gatilho e uma ação
1.	No **Designer de Aplicativos Lógicos**, selecione **Aplicativo Lógico em Branco** na lista **Modelos**.
2.	Na lista de **gatilhos**, selecione **Recorrência**.
3.	No gatilho **Recorrência**, clique em **Editar**, escolha a lista suspensa **Frequência** para selecionar **Dia** e selecione **Intervalo** para digitar **7**.
4.	Selecione a caixa **+ Nova etapa** caixa e escolha **Adicionar uma ação**.
5.	Na lista de **ações**, digite **informix** na caixa de edição **Procurar mais ações** e selecione **Informix - Excluir linha (Visualização)**.
6. Na ação **Obter linhas (Visualização)**, selecione **Alterar conexão**.
7. No painel de configurações de **Conexões**, selecione uma conexão existente. Por exemplo, selecione **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Na lista **Nome da tabela**, selecione a **seta para baixo** e selecione **AREA**.
9. Insira valores para todas as colunas necessárias (confira o asterisco vermelho). Por exemplo, digite `99999` para **AREAID**.
10. Selecione **Salvar**.

	![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)

11.	Na folha **InformixdeleteRow**, na lista **Todas as execuções** em **Resumo**, selecione o item listado primeiro (a execução mais recente).
12.	Na folha **Execução do aplicativo lógico**, escolha **Detalhes da Execução**. Na lista **Ação**, selecione **Get\_rows**. Confira o valor para o **Status**, que deve ser **Êxito**. Selecione o **link Entradas** para exibir as entradas. Selecione o **link Saídas** e exiba as saídas, que deverão incluir a linha excluída.

	![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## Detalhes técnicos

## Ações
Uma ação é uma operação executada pelo fluxo de trabalho definido em um aplicativo lógico. O conector do banco de dados Informix inclui as ações a seguir.

|Ação|Descrição|
|--- | ---|
|[GetRow](connectors-create-api-informix.md#get-row)|Recupera uma única linha de uma tabela Informix|
|[GetRows](connectors-create-api-informix.md#get-rows)|Recupera linhas de uma tabela Informix|
|[InsertRow](connectors-create-api-informix.md#insert-row)|Insere uma nova linha em uma tabela Informix|
|[DeleteRow](connectors-create-api-informix.md#delete-row)|Exclui uma linha de uma tabela Informix|
|[GetTables](connectors-create-api-informix.md#get-tables)|Recupera tabelas de um banco de dados Informix|
|[UpdateRow](connectors-create-api-informix.md#update-row)|Atualiza uma linha existente em uma tabela Informix|

### Detalhes da ação

Nesta seção, consulte os detalhes específicos sobre cada ação, incluindo todas as propriedades de entrada obrigatórias ou opcionais, assim como toda saída correspondente associada ao conector.

#### Obter linha 
Recupera uma única linha de uma tabela Informix.

| Nome da Propriedade| Nome de exibição |Descrição|
| ---|---|---|
|table * | Nome da tabela |Nome da tabela Informix|
|id * | Id da linha |O identificador exclusivo da linha a ser recuperado|

Um asterisco (*) significa que a propriedade obrigatória.

##### Detalhes da Saída
Item

| Nome da Propriedade | Tipo de Dados |
|---|---|
|ItemInternalId|string|


#### Obter linhas 
Recupera linhas de uma tabela Informix.

|Nome da Propriedade| Nome de exibição|Descrição|
| ---|---|---|
|table*|Nome da tabela|Nome da tabela Informix|
|$skip|Ignorar contagem|Número de entradas a serem ignoradas (padrão = 0)|
|$top|Obter Contagem Máxima|Número máximo de entradas a serem recuperadas (padrão = 256)|
|$filter|Consulta de filtro|Uma consulta de filtro ODATA para restringir o número de entradas|
|$orderby|Ordenar por|Uma consulta orderBy do ODATA para especificar a ordem das entradas|

Um asterisco (*) significa que a propriedade obrigatória.

##### Detalhes da Saída
ItemsList

| Nome da Propriedade | Tipo de Dados |
|---|---|
|value|array|


#### Inserir linha 
Insere uma nova linha em uma tabela Informix.

|Nome da Propriedade| Nome de exibição|Descrição|
| ---|---|---|
|table*|Nome da tabela|Nome da tabela Informix|
|item*|Linha|Linha para inserir na tabela especificada no Informix|

Um asterisco (*) significa que a propriedade obrigatória.

##### Detalhes da Saída
Item

| Nome da Propriedade | Tipo de Dados |
|---|---|
|ItemInternalId|string|


#### Excluir linha 
Exclui uma linha de uma tabela Informix.

|Nome da Propriedade| Nome de exibição|Descrição|
| ---|---|---|
|table*|Nome da tabela|Nome da tabela Informix|
|id*|Id da linha|Identificador exclusivo da linha a ser excluída|

Um asterisco (*) significa que a propriedade obrigatória.

##### Detalhes da Saída
Nenhuma.

#### Obter tabelas 
Recupera tabelas de um banco de dados Informix.

Não existem parâmetros para esta chamada.

##### Detalhes da saída 
TablesList

| Nome da Propriedade | Tipo de Dados |
|---|---|
|value|array|

#### Atualizar linha 
Atualiza uma linha existente em uma tabela Informix.

|Nome da Propriedade| Nome de exibição|Descrição|
| ---|---|---|
|table*|Nome da tabela|Nome da tabela Informix|
|id*|Id da linha|Identificador exclusivo da linha a ser atualizada|
|item*|Linha|Linhas com valores atualizados|

Um asterisco (*) significa que a propriedade obrigatória.

##### Detalhes da Saída  
Item

| Nome da Propriedade | Tipo de Dados |
|---|---|
|ItemInternalId|string|


### Respostas HTTP

Ao fazer chamadas a diferentes ações, você pode obter determinadas respostas. A tabela a seguir descreve as respostas e suas descrições:

|Nome|Descrição|
|---|---|
|200|OK|
|202|Aceita|
|400|Solicitação incorreta|
|401|Não Autorizado|
|403|Proibido|
|404|Não encontrado|
|500|Erro Interno do Servidor. Ocorreu um erro desconhecido|
|padrão|Falha na Operação.|


## Versões e plataformas Informix com suporte
Este conector dá suporte às versões do IBM Informix à seguir quando configurado para dar suporte a conexões a conexões de cliente DRDA (Distributed Relational Database Architecture).

- IBM Informix 12.1
- IBM Informix 11.7

 
## Próximas etapas

[Criar um aplicativo lógico](../app-service-logic/app-service-logic-create-a-logic-app.md). Explore os outros conectores disponíveis nos Aplicativos Lógicos em nossa [lista de APIs](apis-list.md).

<!---HONumber=AcomDC_0928_2016-->