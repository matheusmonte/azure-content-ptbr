<properties
    pageTitle="Consulta entre bancos de dados na nuvem com esquemas diferentes | Microsoft Azure"
    description="como configurar consultas entre bancos de dados em partições verticais"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/27/2016"
    ms.author="torsteng" />

# Consultar entre bancos de dados na nuvem com esquemas diferentes (visualização)

![Consultar tabelas em bancos de dados diferentes][1]

Bancos de dados particionados verticalmente usam diferentes conjuntos de tabelas diferentes bancos de dados. Isso significa que o esquema é diferente em bancos de dados diferentes. Por exemplo, todas as tabelas de inventário estão em um banco de dados, enquanto todas as tabelas relacionadas à contabilidade estão em um segundo banco de dados.

## Pré-requisitos

* O usuário deve ter a permissão para ALTERAR QUALQUER FONTE DE DADOS EXTERNA. Essa permissão está incluída na permissão ALTERAR BANCO DE DADOS.
* As permissões para ALTERAR QUALQUER FONTE DE DADOS EXTERNA são necessárias para referenciar a fonte de dados subjacente.

## Visão geral

**OBSERVAÇÃO**: Ao contrário do particionamento horizontal, essas instruções DDL não dependem da definição de uma camada de dados com um mapa de fragmentos por meio da biblioteca de cliente do banco de dados elástico.

1. [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx)
2. [CRIAR UMA CREDENCIAL NO ESCOPO DO BANCO DE DADOS](https://msdn.microsoft.com/library/mt270260.aspx)
3. [CREATE EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx)
4. [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) 


## Criar chave mestra do escopo do banco de dados e credenciais 

A credencial é usada pela consulta elástica para se conectar aos bancos de dados remotos.

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Observação** Verifique se o *<username>* não inclui nenhum sufixo *“@servername”*.

## Criar fontes de dados externas

Sintaxe:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Importante** O parâmetro TYPE deve ser definido **RDBMS**.

### Exemplo 

O exemplo a seguir ilustra o uso da instrução CRIAR para fontes de dados externas.

	CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
	WITH 
	( 
		TYPE=RDBMS, 
		LOCATION='myserver.database.windows.net', 
		DATABASE_NAME='ReferenceData', 
		CREDENTIAL= SqlUser 
	); 
 
Para recuperar a lista de fontes de dados externas atuais:

    select * from sys.external_data_sources; 

### Tabelas externas 

Sintaxe:

	CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
	{ WITH ( <rdbms_external_table_options> ) } 
	)[;] 
	
	<rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### Exemplo  

	CREATE EXTERNAL TABLE [dbo].[customer]( 
		[c_id] int NOT NULL, 
		[c_firstname] nvarchar(256) NULL, 
		[c_lastname] nvarchar(256) NOT NULL, 
		[street] nvarchar(256) NOT NULL, 
		[city] nvarchar(256) NOT NULL, 
		[state] nvarchar(20) NULL, 
		[country] nvarchar(50) NOT NULL, 
	) 
	WITH 
	( 
	       DATA_SOURCE = RemoteReferenceData 
	); 

O exemplo a seguir mostra como recuperar a lista de tabelas externas do banco de dados atual:

	select * from sys.external_tables; 

### Comentários

A consulta elástica estende a sintaxe de tabela externa existente para definir as tabelas externas que usam fontes de dados externas do tipo RDBMS. Uma definição de tabela externa para o particionamento vertical abrange os seguintes aspectos:

* **Esquema**: a DDL da tabela externa define um esquema que pode ser usado pelas consultas. O esquema fornecido na definição da tabela externa precisa corresponder ao esquema das tabelas no banco de dados remoto em que os dados reais são armazenados. 

* **Referência de banco de dados remoto**: a DDL da tabela externa faz referência a uma fonte de dados externa. A fonte de dados externa especifica o nome do servidor lógico e o nome do banco de dados remoto em que os dados da tabela real estão armazenados.

Com uma fonte de dados externa, como descrito na seção anterior, a sintaxe para criar tabelas externas é a seguinte:

A cláusula DATA\_SOURCE define a fonte de dados externa (ou seja, o banco de dados remoto, no caso do particionamento vertical) que é usada para a tabela externa.

As cláusulas SCHEMA\_NAME e OBJECT\_NAME fornecem a capacidade de mapear a definição da tabela externa para uma tabela em um esquema diferente no banco de dados remoto ou para uma tabela com um nome diferente, respectivamente. Isso é útil se você deseja definir uma tabela externa para uma exibição de catálogo ou DMV em seu banco de dados remoto – ou em qualquer outra situação em que o nome da tabela remota já esteja sendo usado localmente.

A instrução DDL a seguir remove uma definição existente da tabela externa do catálogo do local. Ela não afeta o banco de dados remoto.

	DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Permissões para CREATE/DROP EXTERNAL TABLE**: as permissões ALTER ANY EXTERNAL DATA SOURCE são necessárias para a DDL da tabela externa, o que também é necessário para fazer referência à fonte de dados subjacente.

## Considerações de segurança
Usuários com acesso à tabela externa têm acesso automaticamente a tabelas remotas subjacentes com a credencial fornecida na definição de fonte de dados externa. Você deve gerenciar cuidadosamente o acesso à tabela externa para evitar a elevação indesejada de privilégios por meio da credencial da fonte de dados externa. Permissões de SQL regulares podem ser usadas para o acesso de GRANT ou REVOKE a uma tabela externa como se ela fosse uma tabela normal.


## Exemplo: consultando bancos de dados particionados verticalmente 

A consulta a seguir executa uma junção de três vias entre as duas tabelas locais para pedidos e linhas da pedido e a tabela remota para clientes. Este é um exemplo do caso de uso de dados de referência para a consulta elástica:

	SELECT  	
	 c_id as customer,
	 c_lastname as customer_name,
	 count(*) as cnt_orderline, 
	 max(ol_quantity) as max_quantity,
	 avg(ol_amount) as avg_amount,
	 min(ol_delivery_d) as min_deliv_date
	FROM customer 
	JOIN orders 
	ON c_id = o_c_id
	JOIN  order_line 
	ON o_id = ol_o_id and o_c_id = ol_c_id
	WHERE c_id = 100


## Procedimento armazenado para a execução remota de T-SQL: sp\_execute\_remote

A consulta elástica também apresenta um procedimento armazenado que fornece acesso direto aos fragmentos. O procedimento armazenado é chamado [sp\_execute\_remote](https://msdn.microsoft.com/library/mt703714) e pode ser usado para executar procedimentos armazenados remotos ou código T-SQL em bancos de dados remotos. Ele usa os seguintes parâmetros:

* Nome da fonte de dados (nvarchar): o nome da fonte de dados externa do tipo RDBMS. 
* Consulta (nvarchar): a consulta T-SQL a ser executada em cada fragmento. 
* Declaração de parâmetro (nvarchar) - opcional: cadeia de caracteres com definições de tipo de dados para os parâmetros usados no parâmetro Query (como sp\_executesql). 
* Lista de valores de parâmetro - opcional: lista separada por vírgulas de valores de parâmetro (como sp\_executesql).

O sp\_execute\_remote usa a fonte de dados externa fornecida nos parâmetros de invocação para executar a instrução T-SQL especificada nos bancos de dados remotos. Ele usa a credencial da fonte de dados externa para a conexão com o banco de dados do gerenciador de mapa do fragmento e bancos de dados remotos.

Exemplo:

	EXEC sp_execute_remote
		N'MyExtSrc',
		N'select count(w_id) as foo from warehouse' 


  
## Conectividade de ferramentas

É possível usar cadeias de conexão regulares do SQL Server para conectar suas ferramentas de BI e de integração de dados a bancos de dados no servidor do Banco de Dados SQL que têm a consulta elástica habilitada e as tabelas externas definidas. Certifique-se de que o SQL Server tem suporte como uma fonte de dados para a ferramenta. Em seguida, consulte o banco de dados de consulta elástica e suas tabelas externas como qualquer outro banco de dados do SQL Server ao qual você se conectaria com a sua ferramenta.

## Práticas recomendadas 
 
* Verifique se o banco de dados do ponto de extremidade da consulta elástica recebeu acesso ao banco de dados remoto habilitando acesso para os Serviços do Azure em sua configuração de firewall do Banco de Dados SQL. Também verifique se a credencial fornecida na definição da fonte de dados externa pode fazer logon com êxito no banco de dados remoto e se ela tem as permissões para acessar a tabela remota.  

* A consulta elástica funciona melhor para consultas em que a maior parte da computação pode ser realizada nos bancos de dados remotos. Normalmente, você obtém o melhor desempenho de consulta com predicados de filtro seletivo que podem ser avaliados nos bancos de dados remotos ou com junções que podem ser executadas por completo no banco de dados remoto. Outros padrões de consulta podem precisar carregar grandes quantidades de dados do banco de dados remoto e podem ter um desempenho insatisfatório.


## Próximas etapas

Para consultar bancos de dados particionados horizontalmente (também conhecido como bancos de dados fragmentados), consulte [Consultas entre bancos de dados de nuvem fragmentados (particionados horizontalmente)](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->

<!---HONumber=AcomDC_0601_2016-->