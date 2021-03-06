<properties
   pageTitle="Usando o Conector SQL em Aplicativos Lógicos | Serviço de Aplicativo do Microsoft Azure"
   description="Como criar e configurar o Conector SQL ou o aplicativo de API e usá-lo em um aplicativo lógico no Serviço de Aplicativo do Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# Introdução ao Conector do Microsoft SQL e à adição dele ao seu Aplicativo Lógico
>[AZURE.NOTE] Esta versão do artigo aplica-se à versão do esquema 2014-12-01-preview de aplicativos lógicos. Para obter a versão do esquema 2015-08-01-preview do SQL do Azure, clique em [API do SQL Azure](../connectors/connectors-create-api-sqlazure.md).

Conecte-se ao SQL Server local ou a um banco de dados SQL do Azure para criar e alterar suas informações ou dados. Conectores podem ser usados em Aplicativos de Lógicos para obter, processar ou enviar dados como parte de um “fluxo de trabalho”. Utilizando o Conector do SQL em seu fluxo de trabalho, você pode chegar a diversos resultados. Por exemplo, você pode:

- Expor uma seção dos dados residentes no seu banco de dados SQL usando um aplicativo da Web ou móvel.
- Inserir dados em uma tabela de banco de dados SQL para armazenamento. Por exemplo, você pode inserir registros de funcionários, atualizar ordens de venda e assim por diante.
- Extrair dados do SQL para uso em um processo comercial. Por exemplo, você pode obter registros do cliente e colocá-los no SalesForce.

Você pode adicionar o Conector SQL a seu fluxo de trabalho de negócios e processar dados como parte desse fluxo de trabalho dentro de um Aplicativo Lógico.

## Gatilhos e Ações
*Gatilhos* são eventos que ocorrem. Por exemplo, quando um pedido é atualizado ou quando um novo cliente é adicionado. Uma *Ação* é o resultado do gatilho. Por exemplo, quando uma ordem é atualizada, enviar um alerta para o vendedor. Ou, quando um novo cliente for adicionado, enviar um email de boas-vindas para ele.

O conector do SQL pode ser usado como um gatilho ou uma ação em um aplicativo lógico e dá suporte a dados nos formatos JSON e XML. Para cada tabela incluída nas configurações do pacote (falaremos mais sobre isso posteriormente neste tópico), há um conjunto de ações de JSON e um conjunto de ações de XML.

O conector de SQL tem os seguintes gatilhos e ações disponíveis:

Gatilhos | Ações
--- | ---
Sondar dados | <ul><li>Inserir na Tabela</li><li>Atualizar Tabela</li><li>Selecionar da Tabela</li><li>Excluir da Tabela</li><li>Chamar Procedimento Armazenado</li></ul>

## Criar o conector do SQL

Um conector pode ser criado em um aplicativo lógico ou diretamente no Azure Marketplace. Para criar um conector no Marketplace:

1. No quadro inicial do Azure, selecione **Marketplace**.
2. Procure "Conector do SQL", selecione-o e selecione **Criar**.
3. Digite o Nome, o Plano do Serviço de Aplicativo e outras propriedades.
4. Insira as seguintes configurações de pacote:

	Nome | Obrigatório | Descrição
--- | --- | ---
Nome do Servidor | Sim | Especifique o nome do SQL Server. Por exemplo, digite *SQLserver/sqlexpress* ou *SQLserver.mydomain.com*.
Port | Não | O padrão é 1433.
Nome de usuário | Sim | Insira um nome de usuário que possa fazer logon no SQL Server. Caso se conecte a um SQL Server local, insira as credenciais de autenticação do SQL.
Senha | Sim | Digite a senha do nome de usuário.
Nome do Banco de Dados | Sim | Informe o banco de dados ao qual está se conectando. Por exemplo, você pode inserir *Clientes* ou *dbo/pedidos*.
Local | Sim | O padrão é False. Insira False se estiver se conectando a um banco de dados SQL do Azure. Insira True caso esteja se conectando a um SQL Server local.
Cadeia de conexão do Barramento de Serviço | Não | Se você estiver estabelecendo conexão local, insira a cadeia de conexão de retransmissão do Barramento de Serviço.<br/><br/>[Usando o Gerenciador de Conexão Híbrida](app-service-logic-hybrid-connection-manager.md)<br/>[Preços do Barramento de Serviço](https://azure.microsoft.com/pricing/details/service-bus/)
Nome do Servidor Parceiro | Não | Se o servidor primário não estiver disponível, você poderá informar um servidor parceiro como servidor alternativo ou de backup.
Tabelas | Não | Lista as tabelas de banco de dados que podem ser atualizadas pelo conector. Por exemplo, digite *OrdersTable* ou *EmployeeTable*. Se nenhuma tabela for especificada, todas as tabelas poderão ser usadas. Tabelas válidas e/ou procedimentos armazenados são necessários para usar esse conector como uma ação.
Procedimentos Armazenados | Não | Informe um procedimento armazenado existente que pode ser chamado pelo conector. Por exemplo, digite *sp\_IsEmployeeEligible* ou *sp\_CalculateOrderDiscount*. Tabelas válidas e/ou procedimentos armazenados são necessários para usar esse conector como uma ação.
Consulta de Dados Disponíveis | Para suporte de gatilho | Instrução SQL para determinar se há dados disponíveis para sondar uma tabela de banco de dados do SQL Server. Deve retornar um valor numérico que representa o número de linhas de dados disponíveis. Exemplo: SELECT COUNT(*) from table\_name.
Sondar Consulta de Dados | Para suporte de gatilho | Instrução SQL para sondar a tabela de banco de dados do SQL Server. Você pode especificar qualquer número de instruções SQL separadas por ponto e vírgula. Essa instrução é executada transacionalmente e confirmada somente quando os dados são armazenados com segurança em seu aplicativo lógico. Exemplo: SELECT * FROM nome\_da\_tabela; DELETE FROM nome\_da\_tabela. <br/><br/>**Observação**<br/>Você deve fornecer uma instrução de pesquisa que evite um loop infinito. Para isso, exclua, mova ou atualize os dados selecionados para garantir que eles não sejam sondados novamente.

5. Após a conclusão, as Configurações do Pacote serão semelhantes às seguintes:  
![][1]  

6. Selecione **Criar**.


## Usar o Conector como um Gatilho
Vamos examinar um aplicativo lógico simples que sonda os dados de uma tabela SQL, adiciona os dados a outra tabela e os atualiza.

Para usar o conector do SQL como um gatilho, informe os valores de **Consulta de Dados Disponíveis** e **Sondar Consulta de Dados**. **Consulta de Dados Disponíveis** é executada de acordo com o agendamento informado e determina se há dados disponíveis. Como essa consulta retorna apenas um número escalar, ela pode ser ajustada e otimizada para execução frequente.

**Sondar Consulta de Dados** só é executado quando a consulta de dados disponíveis indica que há dados disponíveis. Essa instrução é executada em uma transação e só é confirmada quando os dados extraídos são armazenados permanentemente no fluxo de trabalho. É importante evitar extrair novamente os mesmos dados por tempo indefinido. A natureza transacional dessa execução pode ser usada para excluir ou atualizar os dados a fim de garantir que eles não sejam coletados na próxima consulta.

> [AZURE.NOTE] O esquema retornado por essa instrução identifica as propriedades disponíveis em seu conector. Todas as colunas devem ser nomeadas.

#### Exemplo de Consulta de Dados Disponíveis

	SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### Exemplo de Sondar Consulta de Dados

	SELECT *, GetData() as 'PollTime' FROM [Order]
		WHERE OrderStatus = 'ProcessedForCollection'
		ORDER BY Id DESC;
	UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
		WHERE Id =
		(SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### Adicionar o Gatilho
1. Ao criar ou editar um aplicativo lógico, selecione o conector do SQL criado como gatilho. Isso lista os gatilhos disponíveis: **Sondar Dados (JSON)** e **Sondar Dados (XML)**:  
![][5]

2. Selecione o gatilho **Sondar Dados (JSON)**, insira a frequência e clique em ✓:  
![][6]

3. O gatilho agora aparece conforme configurado no aplicativo lógico. As saídas do gatilho serão mostradas e poderão ser usadas como entradas em quaisquer ações posteriores:  
![][7]

## Usar o Conector como uma Ação
Usaremos o cenário de aplicativo lógico simples que sonda os dados de uma tabela SQL, adiciona os dados a outra tabela e os atualiza.

Para usar o conector do SQL como uma ação, insira o nome de tabelas e/ou procedimentos armazenados que você inseriu quando você criou o conector do SQL:

1. Após o gatilho (ou escolha 'executar esta lógica manualmente'), adicione o conector do SQL criado na galeria. Selecione uma das ações Inserir, como *Inserir em TempEmployeeDetails (JSON)*:  
![][8]

2. Especifique os valores de entrada do registro a ser inserido e clique em ✓:  
![][9]

3. Na galeria, selecione o mesmo conector do SQL que você criou. Como uma ação, selecione a ação Atualizar na mesma tabela, como *Atualizar EmployeeDetails*:  
![][11]

4. Insira os valores de entrada para a ação de atualização e clique em ✓:  
![][12]

Você pode testar o aplicativo lógico adicionando um novo registro na tabela que está sendo sondada.

## O que você pode e não pode fazer

Consulta SQL | Suportado | Sem suporte
--- | --- | ---
Cláusula Where | <ul><li>Operadores: AND, OR, =, <>, <, <=, >, >= e LIKE</li><li>Várias subcondições podem ser combinadas por '(' e ')'</li><li>Literais de cadeia, data/hora (entre aspas simples), números (só devem conter caracteres numéricos)</li><li>Deve estar estritamente em formato de expressão binária, como ((operando operador operando) AND/OR (operando operador operando))*</li></ul> | <ul><li>Operadores: Between, IN</li><li>Todas as funções internas, como ADD(), MAX() NOW(), POWER() e assim por diante</li><li>Operadores matemáticos, como *, -, +, e assim por diante em </li><li>Concatenações de cadeias usando +.</li><li>Todas as associações</li><li>IS NULL e IS NOT Null</li><li>Quaisquer números com caracteres não numéricos, como números hexadecimais</li></ul>
Campos (na consulta Select) | <ul><li>Nomes de coluna válidos separados por vírgulas. Nenhum prefixo de nome de tabela permitido (o conector funciona em uma tabela por vez).</li><li>Os nomes podem ser escapados com '[' e ']'</li></ul> | <ul><li>Palavras-chave como TOP, DISTINCT etc.</li><li>Alias, como Rua + Cidade + Código postal como Endereço</li><li>Todas as funções internas, como ADD(), MAX() NOW(), POWER() etc.</li><li>Operadores matemáticos, como *, -, + etc.</li><li>Concatenações de cadeias de caracteres usando +</li></ul>

#### Dicas

- Para consultas avançadas, sugerimos criar um procedimento armazenado e executá-lo usando a API de execução de procedimento armazenado.
- Use consultas internas em procedimentos armazenados.
- Para unir várias condições, você pode usar os operadores ‘AND’ e ‘OR’.

## Configuração Híbrida (opcional)

> [AZURE.NOTE] Essa etapa será necessária apenas se você estiver usando o SQL Server local por trás do firewall.

O Serviço de Aplicativo usa o Gerenciador de Configuração Híbrida para se conectar com segurança ao sistema local. Se seu conector usar um SQL Server local, o Gerenciador de Conexão Híbrida será necessário.

> [AZURE.NOTE] Se você deseja começar com os Aplicativos Lógicos do Azure antes de se inscrever em uma conta do Azure, acesse [Experimentar os Aplicativos Lógicos](https://tryappservice.azure.com/?appservice=logic), em que você pode criar imediatamente um aplicativo lógico inicial de curta duração no Serviço de Aplicativo. Não é necessário nenhum cartão de crédito; não há compromissos.

Consulte [Usando o Gerenciador de Conexão Híbrida](app-service-logic-hybrid-connection-manager.md).


## Faça mais com seu Conector
Agora que o conector foi criado, você pode adicioná-lo a um fluxo de trabalho comercial usando um Aplicativo Lógico. Consulte [O que são Aplicativos Lógicos?](app-service-logic-what-are-logic-apps.md).

Exibir a referência da API REST de Swagger em [Conectores e referência de aplicativos de API](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Você também pode examinar estatísticas de desempenho e controlar a segurança do conector. Consulte [Gerenciar e monitorar Aplicativos de API e conectores internos](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png

<!---HONumber=AcomDC_0907_2016-->