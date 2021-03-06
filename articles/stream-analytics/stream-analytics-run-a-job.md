<properties 
	pageTitle="Como iniciar trabalhos de transmissão por streaming no Stream Analytics | Microsoft Azure" 
	description="Como executar um trabalho de streaming no Stream Analytics do Azure | segmento do roteiro de aprendizagem."
    keywords="trabalhos de streaming"
	documentationCenter=""
	services="stream-analytics"
	authors="jeffstokes72" 
	manager="jhubbard" 
	editor="cgronlun"/>

<tags 
	ms.service="stream-analytics" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.workload="data-services" 
	ms.date="09/26/2016" 
	ms.author="jeffstok"/>

# Como executar um trabalho de streaming no Stream Analytics do Azure

Quando uma entrada, consulta e saída de trabalho forem especificadas, você poderá iniciar o trabalho do Stream Analytics.

Para iniciar o trabalho:

1.	No portal clássico do Azure, no painel de trabalho, clique em **Iniciar** na parte inferior da página.

    ![Botão Iniciar trabalho](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)

    No portal do Azure, clique em **Iniciar** na parte superior da página de trabalho.

    ![Botão Iniciar trabalho no Portal do Azure](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)

2.	Especifique um valor de **Iniciar Saída** para determinar quando esse trabalho começará a produzir uma saída. A configuração padrão para trabalhos que não foram iniciados anteriormente é **Hora de Início do Trabalho**, o que significa que o trabalho começará imediatamente a processar os dados. Você também pode especificar um tempo **Personalizado** no passado (para o consumo de dados históricos) ou no futuro (para atrasar o processamento até um momento futuro). Para os casos em que um trabalho foi anteriormente iniciado e interrompido, a opção **Hora da Última Interrupção** está disponível para retomar o trabalho da última hora de saída e evitar a perda de dados.

    ![Hora de início do trabalho de streaming](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)

    ![Hora de início do trabalho de transmissão no Portal do Azure](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)

3.	Confirme sua seleção. O status do trabalho será alterado para *Iniciando* e logo mudará para *Em execução* quando o trabalho for iniciado. Você pode monitorar o andamento da operação **Iniciar** no **Hub de Notificação**:

    ![andamento do trabalho de streaming](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)

    ![Andamento do trabalho de transmissão no Portal do Azure](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)

## Obter ajuda
Para obter mais assistência, experimente nosso [Fórum do Stream Analytics do Azure](https://social.msdn.microsoft.com/Forums/pt-BR/home?forum=AzureStreamAnalytics)

## Próximas etapas

- [Introdução ao Stream Analytics do Azure](stream-analytics-introduction.md)
- [Introdução ao uso do Stream Analytics do Azure](stream-analytics-get-started.md)
- [Dimensionar trabalhos do Stream Analytics do Azure](stream-analytics-scale-jobs.md)
- [Referência de Linguagem de Consulta do Stream Analytics do Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Referência da API REST do Gerenciamento do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!---HONumber=AcomDC_0928_2016-->