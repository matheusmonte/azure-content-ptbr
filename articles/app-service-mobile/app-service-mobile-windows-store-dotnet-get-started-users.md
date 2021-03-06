<properties
	pageTitle="Adicionar autenticação ao seu aplicativo da UWP (Plataforma Universal do Windows) | Aplicativos Móveis do Azure"
	description="Aprenda a usar os aplicativos móveis do Serviço de Aplicativos do Azure para autenticar usuários de seu aplicativo da UWP (Plataforma Universal do Windows) usando uma variedade de provedores de identidade, incluindo: AAD, Google, Facebook, Twitter e Microsoft."
	services="app-service\mobile"
	documentationCenter="windows"
	authors="ggailey777"
	manager="erikre"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-windows"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="05/14/2016"
	ms.author="glenga"/>

# Adicionar autenticação ao seu aplicativo do Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Este tópico mostra como adicionar autenticação baseada em nuvem ao seu aplicativo móvel. Neste tutorial, você aprenderá a adicionar autenticação ao projeto de início rápido da UWP (Plataforma Universal do Windows) para aplicativos móveis usando um provedor de identidade com suporte no Serviço de Aplicativo do Azure. Após ser autenticado e autorizado com sucesso pelo back-end do Aplicativo Móvel, o valor da ID de usuário é exibido.

Este tutorial baseia-se no início rápido dos Aplicativos Móveis. Você deve primeiro concluir o tutorial [Introdução aos Aplicativos Móveis](app-service-mobile-windows-store-dotnet-get-started.md).

##<a name="register"></a>Registrar seu aplicativo para autenticação e configurar o Serviço de Aplicativo

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Restringir permissões a usuários autenticados

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Agora, é possível verificar se o acesso anônimo para o back-end foi desabilitado. Com o projeto de aplicativo da UWP configurado como projeto de inicialização, implante e execute o aplicativo; verifique se uma exceção sem tratamento com um código de status 401 (não autorizado) é gerada depois que o aplicativo é iniciado. Isso acontece porque o aplicativo tenta acessar o código do aplicativo móvel como um usuário não autenticado, mas a tabela *TodoItem* agora exige autenticação.

Em seguida, você atualizará o aplicativo para autenticar usuários antes de solicitar recursos do seu Serviço de Aplicativo.

##<a name="add-authentication"></a>Adicionar autenticação ao aplicativo

1. No arquivo de projeto de aplicativo da UWP MainPage.cs, adicione o seguinte trecho de código à classe MainPage:
	
		// Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Esse código autentica o usuário com um logon do Facebook. Se você estiver usando um provedor de identidade além do Facebook, altere o valor **MobileServiceAuthenticationProvider** acima para o valor de seu provedor.

3. Comente ou exclua a chamada para o método **ButtonRefresh\_Click** (ou o método **InitLocalStoreAsync**) na substituição do método **OnNavigatedTo** existente. Isso evita que os dados sejam carregados antes que o usuário seja autenticado. Em seguida, você adicionará um botão **Entrar** ao aplicativo que dispara a autenticação.

4. Adicione o seguinte snippet de código para a classe MainPage:

	    private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
	    {
	        // Login the user and then load data from the mobile app.
	        if (await AuthenticateAsync())
	        {
	            // Switch the buttons and load items from the mobile app.
	            ButtonLogin.Visibility = Visibility.Collapsed;
	            ButtonSave.Visibility = Visibility.Visible;
	            //await InitLocalStoreAsync(); //offline sync support.
	            await RefreshTodoItems();
	        }
	    }
		
5. Abra o arquivo de projeto MainPage.xaml, localize o elemento que define o botão **Salvar** e substitua-o pelo código a seguir:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
				Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Pressione a tecla F5 para executar o aplicativo, clique no botão **Entrar** e entre no aplicativo com o provedor de identidade escolhido. Depois que o seu logon for bem-sucedido, o aplicativo será executado sem erros, e você poderá consultar o seu back-end e fazer atualizações nos dados.


##<a name="tokens"></a>Armazenar o token de autenticação no cliente

O exemplo anterior mostrou uma entrada padrão, que requer que o cliente contate o provedor de identidade e o Serviço de Aplicativo sempre que o aplicativo for iniciado. Além de esse método ser ineficiente, você pode se deparar com problemas relacionados ao uso caso muitos consumidores tentem iniciar o aplicativo ao mesmo tempo. Uma abordagem melhor é armazenar em cache o token de autorização retornado pelo Serviço de Aplicativo e tentar usá-lo antes de usar um logon baseado em provedor.

>[AZURE.NOTE]Você pode armazenar em cache o token emitido pelos Serviços de Aplicativos usando tanto a autenticação gerenciada pelo cliente quanto a autenticação gerenciada pelo serviço. Este tutorial usa a autenticação gerenciada pelo serviço.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##Próximas etapas

Agora que você concluiu este tutorial de autenticação básica, considere continuar com um dos seguintes tutoriais:

+ [Adicionar notificação por push para seu aplicativo](app-service-mobile-windows-store-dotnet-get-started-push.md) Saiba como adicionar suporte a notificações por push para seu aplicativo e configurar seu back-end do aplicativo móvel para usar os Hubs de notificação do Azure para enviar notificações por push.

+ [Habilitar sincronização offline para seu aplicativo](app-service-mobile-windows-store-dotnet-get-started-offline-data.md) Saiba como adicionar suporte offline ao seu aplicativo usando um back-end de Aplicativo Móvel. A sincronização offline permite que os usuários finais interajam com um aplicativo móvel, exibindo, adicionando ou modificando dados, mesmo quando não há conexão de rede.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

<!---HONumber=AcomDC_0629_2016-->