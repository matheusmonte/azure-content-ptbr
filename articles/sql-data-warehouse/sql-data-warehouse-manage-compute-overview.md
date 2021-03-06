<properties
   pageTitle="Gerenciar poder de computação no SQL Data Warehouse do Azure (Visão Geral) | Microsoft Azure"
   description="Funcionalidades de escala horizontal de desempenho no SQL Data Warehouse do Azure. Escale horizontalmente por meio de ajuste de DWUs ou, para economizar custos, pause e retome os recursos de computação."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# Gerenciar poder de computação no SQL Data Warehouse do Azure (Visão Geral)

> [AZURE.SELECTOR]
- [Visão geral](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

A arquitetura do SQL Data Warehouse separa armazenamento e computação, permitindo que cada um seja dimensionado independentemente. Como resultado, você pode escalar horizontalmente o desempenho, economizando custos simultaneamente ao pagar pelo desempenho somente quando necessário.

Esta visão geral descreve os recursos de escala horizontal de desempenho do SQL Data Warehouse e fornece recomendações sobre como e quando usá-los.

- Dimensionar o poder de computação ajustando as [DWUs (unidades de data warehouse)][]
- Pausar ou retomar recursos de computação

<a name="scale-performance-bk"></a>

## Dimensionar o desempenho

No SQL Data Warehouse você pode, com rapidez, escalar horizontalmente o desempenho ou reverter esse processo, aumentando ou diminuindo os recursos de computação de CPU, memória e largura de banda de E/S. Para dimensionar o desempenho, tudo o que você precisa fazer é ajustar o número de [DWUs (unidades de data warehouse)][] que o SQL Data Warehouse aloca para seu banco de dados. O SQL Data Warehouse faz rapidamente a alteração e trata de todas as alterações subjacentes de hardware e software.

Já se foram os dias em que você precisava pesquisar o tipo de processador, a quantidade de memória ou o tipo de armazenamento que são necessários para obter excelente desempenho no seu data warehouse. Ao colocar o Data Warehouse na nuvem, você não precisa mais lidar com problemas de nível baixo de hardware. Em vez disso, o SQL Data Warehouse faz essa pergunta: com que rapidez você deseja analisar seus dados?

### Como dimensiono o desempenho?

Para aumentar ou diminuir elasticamente seu poder de computação, basta alterar a configuração das [DWUs (unidades de data warehouse)][] do banco de dados. O desempenho aumentará linearmente quando você adicionar mais DWU. Em níveis mais altos de DWU, você precisa adicionar mais de 100 DWUs para notar uma melhoria significativa no desempenho. Para ajudar a selecionar saltos significativos em DWUs, oferecemos os níveis DWU que fornecerão os melhores resultados.
 
Para ajustar DWUs, você pode usar qualquer um destes métodos individuais.

- [Dimensionar o poder de computação com o portal do Azure][]
- [Dimensionar o poder de computação com o PowerShell][]
- [Dimensionar o poder de computação com APIs REST][]
- [Dimensionar o poder de computação com o TSQL][]

### Quantas DWUs devo usar?
 
O desempenho no SQL Data Warehouse é em escala linear, e a mudança de uma escala de computação para outra (digamos de 100 DWUs para 2.000 DWUs) acontece em segundos. Isso lhe dá a flexibilidade para fazer experiências com configurações de DWU diferentes até determinar o melhor ajuste para o cenário.

Para entender qual é o valor ideal de DWU é, tente escalar verticalmente, para cima e para baixo, e executar algumas consultas após carregar os dados. Como o dimensionamento é rápido, você pode experimentar vários níveis diferentes de desempenho durante uma hora ou menos. Lembre que o SQL Data Warehouse foi projetado para processar grandes quantidades de dados e para ver suas verdadeiras capacidades para dimensionar, especialmente nas escalas maiores que oferecemos, você desejará usar um grande conjunto de dados que se aproxima ou ultrapassa 1 TB.

Recomendações para encontrar a melhor DWU para sua carga de trabalho:

1. Para um data warehouse em desenvolvimento, comece selecionando um número pequeno de DWUs. Um bom ponto de partida é DW400 ou DW200.
2. Monitore o desempenho do seu aplicativo, observando o número de DWUs selecionadas comparado ao desempenho que você observar.
3. Determine quão rápido ou lento o desempenho deve ser para você obter o nível de desempenho ideal para seus requisitos, presumindo uma escala linear.
4. Aumente ou diminua o número de DWUs proporcionalmente à velocidade desejada do desempenho de sua carga de trabalho. O serviço responderá rapidamente e ajustará os recursos de computação para atender aos novos requisitos de DWU.
5. Continue fazendo ajustes até alcançar um nível de desempenho ideal para seus requisitos de negócios.

### Quando devo dimensionar as DWUs?

Quando você precisar de resultados mais rápidos, aumente suas DWUs e pague por mais desempenho. Quando precisar de menos potência de computação, diminua suas DWUs e pague somente pelo que precisa.

Recomendações para quando dimensionar DWUs:

1. Se o seu aplicativo tiver uma carga de trabalho flutuante, escale ou reduza verticalmente os níveis de DWU a fim de acomodar picos e pontos baixos. Por exemplo, se a carga de trabalho geralmente atinge o pico no fim do mês, planeje a adição de mais DWUs durante esses dias de pico e depois reduza verticalmente quando o período de pico terminar.
2. Antes de executar uma operação de transformação ou carregamento de dados pesados, escale verticalmente as DWUs para que os dados fiquem disponíveis mais rapidamente.

<a name="pause-compute-bk"></a>

## Pausar computação

[AZURE.INCLUDE [Descrição de pausa do SQL Data Warehouse](../../includes/sql-data-warehouse-pause-description.md)]

Para pausar um banco de dados, use qualquer um destes métodos individuais.

- [Pausar computação com o portal do Azure][]
- [Pausar computação com o PowerShell][]
- [Pausar computação com APIs REST][]

<a name="resume-compute-bk"></a>

## Retomar a computação

[AZURE.INCLUDE [Descrição de retomada do SQL Data Warehouse](../../includes/sql-data-warehouse-resume-description.md)]

Para retomar um banco de dados, use qualquer um destes métodos individuais.

- [Retomar computação com o portal do Azure][]
- [Retomar computação com o PowerShell][]
- [Retomar computação com APIs REST][]

## Permissões

Dimensionar o banco de dados exigirá as permissões descritas em [ALTER DATABASE][]. Pausar e Retomar exigirão a permissão [Colaborador do DB SQL][], especificamente Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## Próximas etapas
Consulte os artigos a seguir para ajudar a entender alguns dos principais conceitos de desempenho adicionais:

- [Gerenciamento da carga de trabalho e simultaneidade][]
- [Visão geral do design da tabela][]
- [Distribuição da tabela][]
- [Tabela de indexação][]
- [Partições de tabela][]
- [Estatísticas da tabela][]
- [Práticas recomendadas][]

<!--Image reference-->

<!--Article references-->
[DWUs (unidades de data warehouse)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Dimensionar o poder de computação com o portal do Azure]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Dimensionar o poder de computação com o PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Dimensionar o poder de computação com APIs REST]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Dimensionar o poder de computação com o TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pausar computação com o portal do Azure]: ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pausar computação com o PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pausar computação com APIs REST]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Retomar computação com o portal do Azure]: ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Retomar computação com o PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Retomar computação com APIs REST]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Gerenciamento da carga de trabalho e simultaneidade]: ./sql-data-warehouse-develop-concurrency.md
[Visão geral do design da tabela]: ./sql-data-warehouse-tables-overview.md
[Distribuição da tabela]: ./sql-data-warehouse-tables-distribute.md
[Tabela de indexação]: ./sql-data-warehouse-tables-index.md
[Partições de tabela]: ./sql-data-warehouse-tables-partition.md
[Estatísticas da tabela]: ./sql-data-warehouse-tables-statistics.md
[Práticas recomendadas]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[Colaborador do DB SQL]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0907_2016-->