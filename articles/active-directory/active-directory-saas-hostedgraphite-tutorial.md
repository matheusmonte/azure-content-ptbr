<properties
	pageTitle="Tutorial: integração do Azure Active Directory com o Hosted Graphite | Microsoft Azure"
	description="Saiba como configurar o logon único entre o Azure Active Directory e o Hosted Graphite."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/01/2016"
	ms.author="jeedes"/>


# Tutorial: integração do Azure Active Directory ao Hosted Graphite

O objetivo desse tutorial é mostrar como integrar o Hosted Graphite ao Azure AD (Azure Active Directory).

A integração do Hosted Graphite ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao Hosted Graphite
- Você pode permitir que seus usuários façam logon automaticamente no Hosted Graphite (logon único) com suas contas do Azure AD
- Gerenciar suas contas em um único local: o Portal clássico do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## Pré-requisitos

Para configurar a integração do Azure AD ao Hosted Graphite, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Hosted Graphite com logon único habilitado


> [AZURE.NOTE] Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.


Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).


## Descrição do cenário
O objetivo deste tutorial é permitir que você teste o logon único do Azure AD em um ambiente de teste.

O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do Hosted Graphite por meio da Galeria
2. configurar e testar o logon único do AD do Azure


## Adição do Hosted Graphite por meio da Galeria
Para configurar a integração do Hosted Graphite ao Azure AD, você precisa adicionar o Hosted Graphite da galeria à sua lista de aplicativos de SaaS gerenciados.

**Para adicionar o Hosted Graphite por meio da galeria, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

	![Active Directory][1]

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
	
	![Aplicativos][2]

4. Clique em **Adicionar** na parte inferior da página.
	
	![Aplicativos][3]

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

	![Aplicativos][4]

6. Na caixa de pesquisa, digite **Hosted Graphite**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_01.png)

7. No painel de resultados, selecione **Hosted Graphite** e clique em **Concluir** para adicionar o aplicativo.

	![Seleção do aplicativo na galeria](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_0001.png)

##  configurar e testar o logon único do AD do Azure
O objetivo desta seção é mostrar como configurar e testar o logon único do Azure AD com o Hosted Graphite, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Hosted Graphite é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no Hosted Graphite.

Essa relação de vínculo é estabelecida atribuindo o valor de **nome de usuário** ao Azure AD como sendo o valor de **Nome de usuário** no Hosted Graphite.

Para configurar e testar o logon único do Azure AD com o Hosted Graphite, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o Logon único do AD do Azure](#configuring-azure-ad-single-single-sign-on)**: para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do Azure AD](#creating-an-azure-ad-test-user)** - para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criação de um usuário de teste do Hosted Graphite](#creating-a-hosted-graphite-test-user)**: para ter um equivalente de Brenda Fernandes no Hosted Graphite que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)**: para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)**: para verificar se a configuração funciona.

### Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal clássico e configurará o logon único no aplicativo Hosted Graphite.

**Para configurar o logon único do Azure AD com o Hosted Graphite, execute as seguintes etapas:**

1. No portal clássico, na página de integração de aplicativos do **Hosted Graphite**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.
	 
	![Configurar o logon único][6]

2. Na página **Como você deseja que os usuários façam logon no Hosted Graphite**, selecione **Logon Único do Azure AD** e clique em **Avançar**.
    
	![Configurar o logon único](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_03.png)

3. Na página da caixa de diálogo **Definir Configurações de Aplicativo**, se quiser configurar o aplicativo em **modo iniciado pelo IDP**, execute as seguintes etapas e clique em **Avançar**:

    ![Configurar o logon único](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_04.png)

	a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://www.hostedgraphite.com/metadata/<user id>`

    b. Na caixa de texto **URL de Resposta**, digite uma URL usando o seguinte padrão:`https://www.hostedgraphite.com/complete/saml/<user id>`

	c. Clique em **Próximo**.

4. Se quiser configurar o aplicativo no **modo iniciado pelo SP**, na página de caixa de diálogo **Definir Configurações do Aplicativo**, clique em **"Mostrar configurações avançadas (opcional)"**, insira a **URL de Entrada** e clique em **Avançar**.

	![Configurar o logon único](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)

	a. Na caixa de texto **URL de Entrada**, digite uma URL usando o seguinte padrão: `https://www.hostedgraphite.com/login/saml/<user id>/`

	b. Clique em **Próximo**.

	> [AZURE.NOTE] Observe que esses não são os valores reais. Você precisa atualizar esses valores com a URL de Entrada, o Identificador e a URL de Resposta reais. Para obter esses valores, você pode ir para Acesso-> Configuração do SAML no lado do aplicativo ou entrar em contato com o Hosted Graphite.

5. Na página **Configurar logon único no Hosted Graphite**, execute as seguintes etapas e clique em **Avançar**:

	![Configurar o logon único](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_05.png)

    a. Clique em **Baixar certificado** e salve o arquivo em seu computador.

    b. Clique em **Avançar**.

6. Faça logon no seu locatário do Hosted Graphite como administrador.

7. Acesse a **página de configuração do SAML** na barra lateral (**Acesso -> Configuração do SAML**).

	![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

8. Confirme que esses URls correspondem à configuração na etapa 3.

	![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

9. Copie a **URL do emissor** e a **URL SSO do SAML** do Azure AD para a **ID da entidade ou do emissor** e a **URL de logon SSO** no Hosted Graphite.

	![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)

	![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_003.png)

9. Selecione "**Somente leitura**" como a **Função de usuário padrão**.

	![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

10. Copie o conteúdo do arquivo do certificado baixado e cole-o na caixa de texto **Certificado X.509**.

	 ![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

11. Clique no botão **Salvar**.

12. No portal clássico, selecione a confirmação da configuração de logon único e clique em **Avançar**.
    
	![Logon único do AD do Azure][10]

13. Na página **Confirmação de logon único**, clique em **Concluir**.
    
	![Logon único do AD do Azure][11]



### Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal Clássico do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal Clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_09.png)

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.
    
	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png)

4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png)

5. Na página de caixa de diálogo **Conte-nos sobre este usuário**, execute as seguintes etapas:

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_05.png)

    a. Em Tipo de Usuário, selecione Novo usuário na organização.

    b. Na **caixa de texto** de Nome de usuário, digite **BrittaSimon**.

    c. Clique em **Avançar**.

6.  Na página de caixa de diálogo **Perfil do Usuário**, execute as seguintes etapas:
    
	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_06.png)

    a. Na caixa de texto **Nome**, digite **Brenda**.

    b. Na caixa de texto **Sobrenome**, digite **Fernandes**.

    c. Na caixa de texto **Nome de exibição**, digite **Brenda Fernandes**.

    d. Na lista **Função**, selecione **Usuário**.

    e. Clique em **Avançar**.

7. Na página de caixa de diálogo **Obter senha temporária**, clique em **criar**.
    
	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_07.png)

8. Na página de caixa de diálogo **Obter senha temporária**, execute as seguintes etapas:
    
	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_08.png)

    a. Anote o valor da **Nova Senha**.

    b. Clique em **Concluído**.



### Criação de um usuário de teste no Hosted Graphite

O objetivo desta seção é criar uma usuária chamada Brenda Fernandes no Hosted Graphite. O Hosted Graphite dá suporte ao provisionamento Just-In-Time, que está habilitado por padrão.

Não há itens de ação para você nesta seção. Um novo usuário será criado durante uma tentativa de acessar o Hosted Graphite, caso ele ainda não exista.

> [AZURE.NOTE] Se precisar criar um usuário manualmente, entre em contato com a equipe de suporte do Hosted Graphite pelo endereço <mailto:help@hostedgraphite.com>.


### Atribuição do usuário de teste do AD do Azure

O objetivo desta seção é habilitar Brenda Fernandes a usar o logon único do Azure, concedendo a ela acesso ao Hosted Graphite.
	
![Atribuir usuário][200]

**Para atribuir Brenda Fernandes ao Hosted Graphite, execute as seguintes etapas:**

1. No portal clássico, para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **Aplicativos** no menu superior.
    
	![Atribuir usuário][201]

2. Na lista de aplicativos, selecione **Hosted Graphite**.
    
	![Configurar o logon único](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_50.png)

1. No menu na parte superior, clique em **Usuários**.
    
	![Atribuir usuário][203]

1. Na lista Usuários, selecione **Brenda Fernandes**.

2. Na barra de ferramentas na parte inferior, clique em **Atribuir**.
    
	![Atribuir usuário][205]



### Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.
 
Ao clicar no bloco Hosted Graphite no Painel de Acesso, você deverá ser conectado automaticamente no seu aplicativo Hosted Graphite.


## Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0810_2016-->