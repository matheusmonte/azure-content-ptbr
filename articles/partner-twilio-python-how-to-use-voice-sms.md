<properties
	pageTitle="Como usar o Twilio para voz e SMS (PHP) | Microsoft Azure"
	description="Saiba como fazer uma chamada telefônica e enviar uma mensagem SMS com o serviço de API do Twilio no Azure. Exemplos de código escritos em PHP."
	services=""
	documentationCenter="python"
	authors="devinrader"
	manager="twilio"
	editor=""/>

<tags
	ms.service="multiple"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="python" 
	ms.topic="article"
	ms.date="02/19/2015"
	ms.author="MicrosoftHelp@twilio.com"/>





# Como usar o Twilio para obter recursos de voz e SMS no PHP
Este guia demonstra como executar tarefas comuns de programação com o serviço de API do Twilio no Azure. Os cenários abrangidos incluem fazer uma chamada telefônica e enviar uma mensagem serviço de mensagem curta (SMS). Para obter mais informações sobre o Twilio e o uso de voz e SMS em seus aplicativos, consulte a seção [Próximas etapas](#NextSteps).

## <a id="WhatIs"></a>O que é Twilio?
Twilio é alimentar o futuro das comunicações de negócios, que permite aos desenvolvedores incorporar voz, VoIP e mensagens em aplicativos. Eles virtualizam toda a infra-estrutura necessária em um ambiente baseado em nuvem, global, expondo-as por meio da plataforma de comunicações API Twilio. Os aplicativos são simples de criar e dimensionável. Aproveite a flexibilidade com pagamento medida que vá preços e se beneficiar da confiabilidade da nuvem.

**Twilio Voice** permite que seus aplicativos façam e recebam chamadas telefônicas. O **SMS da Twilio** permite que seu aplicativo envie e receba mensagens de texto. **Cliente de Twilio** permite que você faça chamadas VoIP de qualquer telefone, tablet ou navegador e oferece suporte a WebRTC.

## <a id="Pricing"></a>Preços e ofertas especiais da Twilio

Os clientes do Azure recebem uma [oferta especial] de US$ 10 de crédito do Twilio quando você atualiza sua conta do Twilio. Esse crédito de Twilio pode ser aplicado a qualquer uso de Twilio (equivalente a até 1.000 mensagens SMS de envio ou recebimento de até 1000 minutos de voz entrados, dependendo da localização do seu destino de chamada e mensagem ou número de telefone de crédito de US $10). Resgate esse crédito do Twilio e comece a usar em: [ahoy.twilio.com/azure].

Twilio é um serviço flexível. Não há nenhuma taxa de configuração e você pode fechar sua conta a qualquer momento. Você pode encontrar mais detalhes em [Preços da Twilio][twilio_pricing].

## <a id="Concepts"></a>Conceitos
A API do Twilio é uma API RESTful que fornece os recursos de voz e SMS para aplicativos. As bibliotecas de cliente estão disponíveis em vários idiomas. Para obter uma lista, consulte [Bibliotecas de API da Twilio][twilio_libraries].

Principais aspectos da API do Twilio são Twilio verbos e linguagem de marcação de Twilio (TwiML).

### <a id="Verbs"></a>Verbos da Twilio
A API usa os verbos da Twilio; por exemplo, o verbo **&lt;Say&gt;** instrui a Twilio a fornecer de forma audível uma mensagem em uma chamada.

A seguir está uma lista de verbos do Twilio. Saiba mais sobre os outros verbos e recursos na Documentação da linguagem de marcação do Twilio] [http://www.twilio.com/docs/api/twiml].

* **&lt;Dial&gt;**: Conecta o chamador a outro telefone.
* **&lt;Gather&gt;**: Coleta os dígitos numéricos inseridos no teclado numérico do telefone.
* **&lt;Hangup&gt;**: Termina uma chamada.
* **&lt;Play&gt;**: Reproduz um arquivo de áudio.
* **&lt;Pause&gt;**: Espera silenciosamente por um número especificado de segundos.
* **&lt;Registro&gt;**: grava a voz do chamador e retorna um URL de um arquivo que contém a gravação.
* **&lt;Redirect&gt;**: Transfere o controle de uma chamada ou SMS para TwiML em uma URL diferente.
* **&lt;Reject&gt;**: Descarta uma chamada recebida para o número da Twilio sem cobrar você
* **&lt;Say&gt;**: Converte o texto em fala que é executada em uma chamada.
* **&lt;Sms&gt;**: Envia uma mensagem SMS.

### <a id="TwiML"></a>TwiML
TwiML é um conjunto de instruções em XML com base nos verbos do Twilio que informam o Twilio como processar uma chamada ou SMS.

Por exemplo, o seguinte TwiML converteria a mensagem **Olá, mundo** em fala.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Quando o aplicativo chama a API Twilio, um dos parâmetros a API é a URL que retorna a resposta TwiML. Para fins de desenvolvimento, você pode usar URLs fornecidos Twilio para fornecer as respostas de TwiML usadas por seus aplicativos. Você também pode hospedar seus próprio URLs para produzir as respostas TwiML e outra opção é usar o **TwiMLResponse** objeto.

Para obter mais informações sobre os verbos da Twilio, seus atributos e a TwiML, consulte [TwiML][twiml]. Para obter informações adicionais sobre a API da Twilio, consulte [API da Twilio][twilio_api].

## <a id="CreateAccount"></a>Criar uma conta na Twilio
Quando você estiver pronto para obter uma conta do Twilio, inscreva-se em [Experimentar o Twilio][try_twilio]. Você pode começar com uma conta gratuita e atualizá-la depois.

Ao se inscrever em uma conta do Twilio, você receberá uma ID de conta e um token de autenticação. Eles serão necessários para fazer chamadas de API do Twilio. Para evitar o acesso não autorizado em sua conta, mantenha o token da autenticação seguro. A ID da sua conta e o token de autenticação podem ser exibidos na [página da conta da Twilio][twilio_account], nos campos rotulados **ACCOUNT SID** e **AUTH TOKEN**, respectivamente.

## <a id="create_app"></a>Criar um aplicativo PHP
Um aplicativo PHP que usa o serviço do Twilio e está em execução no Azure não é diferente de qualquer outro aplicativo PHP que usa o serviço do Twilio. Embora os serviços da Twilio sejam baseados em REST e possam ser chamados no PHP de várias maneiras, este artigo se concentra em como usar os serviços da Twilio com a [Biblioteca da Twilio para PHP][twilio_php] no GitHub. Para obter mais informações sobre como usar a biblioteca da Twilio para PHP, consulte [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Instruções detalhadas sobre como criar e implantar um aplicativo Twilio/PHP no Azure estão disponíveis em [Como fazer uma chamada telefônica usando a Twilio em um aplicativo PHP no Azure][howto_phonecall_php].

## <a id="configure_app"></a>Configurar seu aplicativo para usar bibliotecas Twilio
Você pode configurar seu aplicativo para usar a biblioteca do Twilio para PHP de duas maneiras:

1. Baixar a biblioteca da Twilio para PHP do GitHub ([https://github.com/twilio/twilio-php][twilio_php]) e adicionar o diretório **Serviços** ao seu aplicativo.

	-OU-

2. Instalar a biblioteca da Twilio para PHP como um pacote PEAR. Ela pode ser instalada com os seguintes comandos:

		$ pear channel-discover twilio.github.com/pear
		$ pear install twilio/Services_Twilio

Depois de instalar a biblioteca da Twilio para PHP, você pode adicionar uma instrução **require\_once** na parte superior dos arquivos PHP para fazer referência à biblioteca:

    	require_once 'Services/Twilio.php';

Para obter mais informações, consulte [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Como fazer uma chamada externa
Abaixo é mostrado como fazer uma chamada externa usando a classe **Services\_Twilio**. Esse código também usa um site fornecido pelo Twilio para retornar a resposta TwiML (Linguagem de Marcação do Twilio). Substitua os valores dos números telefônico **De** e **Para** e certifique-se de verificar o número telefônico **De** para sua conta do Twilio antes de executar o código.

	// Include the Twilio PHP library.
	require_once 'Services/Twilio.php';

	// Library version.
	$version = "2010-04-01";

	// Set your account ID and authentication token.
	$sid = "your_twilio_account_sid";
	$token = "your_twilio_authentication_token";

	// The number of the phone initiating the the call.
	// (Must be previously validated with Twilio.)
	$from_number = "NNNNNNNNNNN";

	// The number of the phone receiving call.
	$to_number = "NNNNNNNNNNN";

	// Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

	// The phone message text.
	$message = "Hello world.";

	// Create the call client.
	$client = new Services_Twilio($sid, $token, $version);

	//Make the call.
	try
	{
		$call = $client->account->calls->create(
			$from_number,
			$to_number,
  			$url.'?Message='.urlencode($message)
		);
	}
	catch (Exception $e)
	{
		echo 'Error: ' . $e->getMessage();
	}

Como mencionado, esse código utiliza um site fornecido pelo Twilio para retornar a resposta de TwiML. Em vez disso, você pode usar seu próprio site para fornecer a resposta TwiML. Para obter mais informações, consulte [Como fornecer respostas TwiML em seu próprio site](#howto_provide_twiml_responses).


- **Nota**: para solucionar problemas de erros de validação do certificado SSL, consulte [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Como enviar uma mensagem de SMS
Abaixo é mostrado como enviar uma mensagem SMS usando a classe **Services\_Twilio**. O **de** número é fornecido por Twilio para contas de avaliação para enviar mensagens SMS. O número **Para** deve ser verificado para sua conta de Twilio antes de executar o código.

	// Include the Twilio PHP library.
	require_once 'Services/Twilio.php';

	// Library version.
	$version = "2010-04-01";

	// Set your account ID and authentication token.
	$sid = "your_twilio_account_sid";
	$token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
	$to_number = "NNNNNNNNNNN";
	$message = "Hello world.";

	// Create the call client.
	$client = new Services_Twilio($sid, $token, $version);

	// Send the SMS message.
	try
	{
		$client->account->sms_messages->create($from_number, $to_number, $message);
	}
	catch (Exception $e)
	{
		echo 'Error: ' . $e->getMessage();
	}

## <a id="howto_provide_twiml_responses"></a>Como fornecer respostas TwiML de seu próprio site
Quando o aplicativo inicia uma chamada para a API do Twilio, o Twilio enviará a solicitação a uma URL que deve retornar uma resposta TwiML. O exemplo acima usa a URL fornecida pelo Twilio [http://twimlets.com/message][twimlet_message_url]. (Embora o TwiML tenha sido criado para uso do Twilio, você pode exibi-lo em seu navegador. Por exemplo, clique em [http://twimlets.com/message][twimlet_message_url] para ver um elemento `<Response>` vazio. Como outro exemplo, clique em [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] para ver um elemento `<Response>` que contém um elemento `<Say>`.)

Em vez de contar com a URL fornecida pelo Twilio, você pode criar seu próprio site que retorne respostas HTTP. Você pode criar o site em qualquer linguagem que retorne respostas XML. Este tópico pressupõe que você estará usando PHP para criar o TwiML.

A página PHP seguinte resulta em uma resposta de TwiML que diz **Hello World** na chamada.

    <?php
		header("content-type: text/xml");
		echo "<?xml version="1.0" encoding="UTF-8"?>\n";
	?>
	<Response>
		<Say>Hello world.</Say>
	</Response>

Como você pode ver no exemplo acima, a resposta de TwiML é simplesmente um documento XML. A biblioteca da Twilio para PHP contém classes que geram TwiML para você. O exemplo a seguir produz a resposta equivalente conforme mostrado acima, mas usa a classe **Services\_Twilio\_Twiml** na biblioteca da Twilio para PHP:

	require_once('Services/Twilio.php');

	$response = new Services_Twilio_Twiml();
	$response->say("Hello world.");
	print $response;

Para obter mais informações sobre TwiML, consulte [https://www.twilio.com/docs/api/twiml][twiml_reference].

Uma vez que a página PHP estiver configurada para fornecer respostas TwiML, use a URL da página PHP como a URL passada para o método `Services_Twilio->account->calls->create`. Por exemplo, se você tiver um aplicativo da Web chamado **MyTwiML** implantado em um serviço hospedado do Azure, e o nome da página PHP for **mytwiml.php**, a URL poderá ser passada para **Services\_Twilio->account->calls->create** conforme mostrado no exemplo a seguir:

	require_once 'Services/Twilio.php';

	$sid = "your_twilio_account_sid";
	$token = "your_twilio_authentication_token";
	$from_number = "NNNNNNNNNNN";
	$to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

	$client = new Services_Twilio($sid, $token, "2010-04-01");

	try
	{
		$call = $client->account->calls->create(
			$from_number,
			$to_number,
  			$url.'?Message='.urlencode($message)
		);
	}
	catch (Exception $e)
	{
		echo 'Error: ' . $e->getMessage();
	}

Para obter informações adicionais sobre como usar a Twilio no Azure com PHP, consulte [Como fazer uma chamada telefônica usando a Twilio em um aplicativo PHP no Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Como Usar os Serviços Adicionais do Twilio
Além dos exemplos mostrados aqui, o Twilio oferece APIs baseadas na Web que podem ser usadas para aproveitar a funcionalidade adicional do Twilio do aplicativo Azure. Para obter detalhes completos, consulte a [Documentação da API do Twilio][twilio_api_documentation].

## <a id="NextSteps"></a>Próximas etapas
Agora que você aprendeu as noções básicas do serviço Twilio, siga estes links para saber mais:

* [Diretrizes de segurança do Twilio][twilio_security_guidelines]
* [Código de Exemplo e Guias de Procedimentos do Twilio][twilio_howtos]
* [Tutoriais do Guia de início rápido do Twilio][twilio_quickstarts]
* [Twilio no GitHub][twilio_on_github]
* [Fale com o suporte do Twilio][twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]: https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart

<!---HONumber=Oct15_HO3-->