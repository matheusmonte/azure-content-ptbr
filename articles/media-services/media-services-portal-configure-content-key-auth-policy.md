<properties 
	pageTitle="Configurar política de autorização de chave de conteúdo usando o portal do Azure | Microsoft Azure" 
	description="Saiba como configurar uma política de autorização para uma chave de conteúdo." 
	services="media-services" 
	documentationCenter="" 
	authors="juliako" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
 	ms.date="09/19/2016" 
	ms.author="juliako"/>



#Configurar a Política de Autorização de Chave de Conteúdo
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##Visão geral

Os Serviços de Mídia do Microsoft Azure permitem distribuir fluxos MPEG-DASH, Smooth Streaming e HLS (HTTP-Live-Streaming) protegidos com a criptografia AES (usando chaves de criptografia de 128 bits) ou com o [DRM do Microsoft PlayReady](https://www.microsoft.com/playready/overview/). O AMS também permite distribuir fluxos DASH criptografados com o DRM do Widevine. PlayReady e Widevine são criptografados de acordo com a especificação de criptografia comum (ISO/IEC 23001-7 CENC).

Os Serviços de Mídia também fornecem um **Serviço de Entrega de Chaves/Licenças** por meio do qual os clientes podem obter chaves AES ou licenças do PlayReady/Widevine para reproduzir o conteúdo criptografado.

Este tópico mostra como usar o **Portal Clássico do Azure** para configurar a política de autorização de chave de conteúdo. A chave pode ser usada posteriormente para criptografar dinamicamente o conteúdo. Observe que, no momento, você pode criptografar o seguintes formatos de streaming: HLS, MPEG DASH, e Smooth Streaming. Você não pode criptografar o formato de streaming HDSou fazer o download progressivo.

Quando um player solicita um fluxo definido para ser criptografado dinamicamente, os Serviços de Mídia usam a chave configurada para criptografar dinamicamente o conteúdo usando a criptografia AES ou DRM. Para descriptografar o fluxo, o player solicitará a chave do serviço de distribuição de chaves. Para decidir se o usuário está autorizado para obter a chave ou não, o serviço avalia as políticas de autorização que você especificou para a chave.


Se você pretende ter várias chaves de conteúdo ou se deseja especificar uma URL de **Serviço de Entrega de Chave/Licença** diferente do serviço de entrega de chaves dos Serviços de Mídia, use o SDK do .NET dos Serviços de Mídia ou as APIs REST.

[Configurar política de autorização de chave de conteúdo usando o SDK do .NET dos Serviços de Mídia](media-services-dotnet-configure-content-key-auth-policy.md)

[Configurar política de autorização de chave de conteúdo usando a API REST dos Serviços de Mídia](media-services-rest-configure-content-key-auth-policy.md)

###Algumas considerações se aplicam:

- Para poder usar o empacotamento dinâmico e a criptografia dinâmica, verifique se você tem pelo menos uma unidade de streaming reservada. Para obter mais informações, consulte [Como dimensionar um serviço de mídia](media-services-portal-manage-streaming-endpoints.md).
- O ativo deve conter um conjunto de MP4s de taxa de bits adaptável ou arquivos de Smooth Streaming de taxa de bits adaptável. Para obter mais informações, consulte [Codificar um ativo](media-services-encode-asset.md).
- O serviço de entrega de chave armazena em cache ContentKeyAuthorizationPolicy e seus objetos relacionados (opções e restrições da política) por 15 minutos. Se você criar um ContentKeyAuthorizationPolicy e optar por usar uma restrição "Token", testá-lo e, em seguida, atualizar a política de restrição "Aberta", levará aproximadamente 15 minutos antes da política alternar para a versão "Aberta" da política.


##Como: configurar a política de autorização da chave

Para configurar a política de autorização da chave, selecione a página **PROTEÇÃO DE CONTEÚDO**.

Os serviços de mídia oferecem suporte a várias maneiras de autenticar os usuários que fazem solicitações de chave. A política de autorização de chave de conteúdo pode ter restrições de autorização **abertas**, de **token**, ou de **IP** (a de **IP** pode ser configurada com REST ou SDK do .NET).

###Restrição aberta

A restrição **aberta** significa que o sistema fornecerá a chave para qualquer pessoa que fizer uma solicitação de chave. Essa restrição pode ser útil para fins de teste.

![OpenPolicy][open_policy]

###Restrição de token

Para escolher a política com restrição de token, pressione o botão **TOKEN**.

A política restrita do **token** deve ser acompanhada por um token emitido por um **Secure Token Service** (STS). Os Serviços de Mídia dão suporte a tokens no formato [SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (**Tokens Web Simples**) e no formato JWT (**Token Web JSON**). Para obter informações, consulte [Autenticação de token JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Os Serviços de Mídia não fornecem **Serviços de Token Seguro**. Você pode criar um STS personalizado ou usar o Microsoft Azure ACS para emitir tokens. O STS deve ser configurado para criar um token assinado com as a chave especificada e declarações de emissão que você especificou na configuração de restrição do token. O serviço de distribuição de chaves dos serviços de mídia retornará a chave de criptografia para o cliente se o token for válido e as declarações no token corresponderem aos configurados para a chave de conteúdo. Para saber mais, consulte [Uso do Azure ACS para emitir tokens](http://mingfeiy.com/acs-with-key-services).

Ao configurar a política com restrição de **TOKEN**, você deve definir valores para **chave de verificação**, **emissor** e **audiência**. A chave de verificação primária contém a chave que o token foi assinado, o emissor é o serviço de token seguro que emite o token. A audiência (às vezes chamada de escopo) descreve a intenção do token ou o recurso que o token autoriza o acesso. O serviço de distribuição de chaves dos serviços de mídia valida que esses valores no token correspondem aos valores no modelo.

###PlayReady

Ao proteger o conteúdo com **PlayReady**, uma das coisas que você precisa especificar na sua política de autorização é uma cadeia de caracteres XML que define o modelo de licença do PlayReady. Por padrão, a seguinte política é definida:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1"> <LicenseTemplates> <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices> <ContentKey i:type="ContentEncryptionKeyFromHeader" /> <LicenseType>Nonpersistent</LicenseType> <PlayRight> <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput> </PlayRight> </PlayReadyLicenseTemplate> </LicenseTemplates> </PlayReadyLicenseResponseTemplate>

Você pode clicar no botão **Importar a política XML** e fornecer um XML diferente que está em conformidade com o esquema XML definido [aqui](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##Próxima etapa

Revise os roteiros de aprendizagem dos Serviços de Mídia.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornecer comentários

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

<!---HONumber=AcomDC_0921_2016-->