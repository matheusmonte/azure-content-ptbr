<properties
   pageTitle="Resolver alertas de integridade do Endpoint Protection na Central de segurança do Azure| Microsoft Azure"
   description="Este documento mostra como implementar a recomendação da Central de Segurança do Azure para **Resolver alertas de integridade do Endpoint Protection**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# Resolver alertas de integridade do Endpoint Protection na Central de segurança do Azure

A Central de Segurança do Azure recomendará que você resolva os alertas de integridade do Endpoint Protection detectados. A Central de Segurança permite que você veja quais máquinas virtuais (VMs) têm falhas do Endpoint Protection e saiba a quantidade de falhas.

> [AZURE.NOTE] Este documento apresenta o serviço usando uma implantação de exemplo. Ela não é um guia passo a passo.

## Implementar a recomendação

1. Na **folha Recomendações**, selecione **Resolver alertas de integridade do Endpoint Protection**. ![Resolver alertas de integridade do Endpoint Protection][1]

2. Isso abrirá a folha **Falha no Endpoint Protection**, que lista as VMs com falhas e o número de falhas para cada VM. Selecione uma VM na lista. ![Falha no Endpoint Protection][2]

3. Uma folha **Lista de Falhas** é aberta para a VM selecionada, exibindo uma lista de falhas. Selecione uma falha na lista para obter mais informações. Isso abre uma folha com informações sobre a falha selecionada. ![Lista de falhas][3] ![Evento de falha][4]

## Consulte também

Para saber mais sobre a Central de Segurança, confira o seguinte:

- [Configurando políticas de segurança na Central de Segurança do Azure](security-center-policies.md): saiba como configurar políticas de segurança para suas assinaturas e grupos de recursos do Azure.
- [Gerenciar as recomendações de segurança na Central de Segurança do Azure](security-center-recommendations.md): saiba como as recomendações ajudam a proteger os recursos do Azure.
- [Monitoramento de integridade de segurança na Central de Segurança do Azure](security-center-monitoring.md): saiba como monitorar a integridade dos recursos do Azure.
- [Gerenciar e responder aos alertas de segurança na Central de Segurança do Azure](security-center-managing-and-responding-alerts.md): aprenda a gerenciar e responder aos alertas de segurança.
- [Monitorar as soluções de parceiros com a Central de Segurança do Azure](security-center-partner-solutions.md): saiba como monitorar o status de integridade de suas soluções de parceiros.
- [Perguntas frequentes sobre a Central de Segurança do Azure](security-center-faq.md): encontre perguntas frequentes sobre como usar o serviço.
- [Blog de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/): obtenha as últimas notícias de segurança e informações do Azure.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png

<!---HONumber=AcomDC_0803_2016-->