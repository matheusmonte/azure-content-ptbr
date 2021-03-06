<properties 
	pageTitle="Visão geral do SDK do Windows Phone Silverlight" 
	description="Visão geral do SDK do Windows Phone Silverlight para o Mobile Engagement do Azure" 					
	services="mobile-engagement" 
	documentationCenter="mobile" 
	authors="piyushjo" 
	manager="dwrede"
	editor="" />

<tags 
	ms.service="mobile-engagement" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="mobile-windows-phone" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/19/2016" 
	ms.author="piyushjo" />

#Visão geral do SDK do Windows Phone Silverlight para o Mobile Engagement do Azure

Comece aqui para obter todos os detalhes sobre como integrar o Mobile Engagement do Azure em um aplicativo do Windows Phone Silverlight. Se você gostaria de experimentá-lo primeiro, faça nosso [tutorial de 15 minutos](mobile-engagement-windows-phone-get-started.md).

Clique para ver o [Conteúdo do SDK](mobile-engagement-windows-phone-sdk-content.md)

##Procedimentos de integração

1. Comece aqui: [Como integrar o Mobile Engagement em seu aplicativo do Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)

2. Para Notificações: [Como integrar o Reach (Notificações) em seu aplicativo do Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Implementação do plano de marcação: [Como usar a API de marcação avançada do Mobile Engagement em seu aplicativo do Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md).

##Notas de versão

###3\.3.0 (19/04/2016)
Parte do pacote nuget *MicrosoftAzure.MobileEngagement* **v3.4.0**

-   Adicionada a API "TestLogLevel" para habilitar/desabilitar/filtrar logs de console emitidos pelo SDK.

Para a versão anterior, consulte as [notas de versão completas](mobile-engagement-windows-phone-release-notes.md)

##Procedimentos de atualização

Se você já tiver integrado uma versão anterior do SDK no seu aplicativo, você deve considerar os seguintes pontos ao atualizar o SDK.

Você precisará seguir vários procedimentos se perdeu várias versões do SDK. Ver todos os [Procedimentos de atualização](mobile-engagement-windows-phone-upgrade-procedure.md). Por exemplo, se você migrar do 0.10.1 para 0.11.0 você tem que primeiro seguir o procedimento "de 0.9.0 a 0.10.1” e depois o procedimento "de 0.10.1 a 0.11.0".

###De 2.0.0 a 3.3.0

####Logs de teste

Agora, os logs do console produzidos pelo SDK podem ser habilitados/desabilitados/filtrados. Para personalizar esse recurso, atualize a propriedade `EngagementAgent.Instance.TestLogEnabled` para um dos valores disponíveis na enumeração `EngagementTestLogLevel`, por exemplo:

			EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
			EngagementAgent.Instance.Init();

### Atualizar de versões anteriores

Consulte [Procedimentos de atualização](mobile-engagement-windows-phone-upgrade-procedure.md)
 

<!---HONumber=AcomDC_0824_2016-->