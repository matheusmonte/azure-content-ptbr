<properties
	pageTitle="Tutorial: Integração do Azure Active Directory com o YouEarnedIt | Microsoft Azure"
	description="Saiba como configurar o logon único entre o Azure Active Directory e o YouEarnedIt."
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
	ms.date="07/18/2016"
	ms.author="jeedes"/>


# Tutorial: Integração do Azure Active Directory com o YouEarnedIt

Neste tutorial, você aprenderá a integrar o YouEarnedIt ao Azure AD (Azure Active Directory).

A integração do YouEarnedIt ao Azure AD oferece os seguintes benefícios:

- No Azure AD, você poderá controlar quem tem acesso ao YouEarnedIt
- Você pode permitir que os usuários façam logon automaticamente no YouEarnedIt (Logon Único) com suas contas do Azure AD
- Gerenciar suas contas em um único local: o Portal clássico do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## Pré-requisitos

Para configurar a integração do Azure AD com o YouEarnedIt, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do YouEarnedIt


> [AZURE.NOTE] Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.


Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).


## Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste.

O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o YouEarnedIt da galeria
2. Configurar e testar o logon único do AD do Azure


## Adicionando o YouEarnedIt da galeria
Para configurar a integração do YouEarnedIt ao Azure AD, você precisará adicionar o YouEarnedIt da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o YouEarnedIt por meio da galeria, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

	![Active Directory][1]

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

	![Aplicativos][2]

4. Clique em **Adicionar** na parte inferior da página.

	![Aplicativos][3]

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

	![Aplicativos][4]

6. Na caixa de pesquisa, digite **YouEarnedIt**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_01.png)

7. No painel de resultados, selecione **YouEarnedIt** e clique em **Concluir** para adicionar o aplicativo.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_06.png)


##  Configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o YouEarnedIt, com base em uma usuária de teste chamada "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do YouEarnedIt é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do YouEarnedIt.

Essa relação de vínculo é estabelecida atribuindo o valor do **nome de usuário** no Azure AD ao valor do **Nome de Usuário** no YouEarnedIt.

Para configurar e testar o logon único do AD do Azure com o YouEarnedIt, você precisará concluir os seguintes blocos de construção:

1. **[Configurar o Logon único do AD do Azure](#configuring-azure-ad-single-sign-on)**: para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)**: para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criar um usuário de teste do YouEarnedIt](#creating-a-predictix-price-reporting-test-user)** - para ter um equivalente de Brenda Fernandes no YouEarnedIt que esteja vinculado à representação dela no AD do Azure.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)**: para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)**: para verificar se a configuração funciona.

### Configuração do logon único do AD do Azure

Nesta seção, você habilitará o logon único do Azure AD no portal clássico e configurará o logon único em seu aplicativo do YouEarnedIt.


**Para configurar o logon único do Azure AD com o YouEarnedIt, execute as seguintes etapas:**

1. No portal clássico do Azure, na página de integração do aplicativo **YouEarnedIt**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.
	 
	![Configurar o logon único][6]

2. Na página **Como você deseja que os usuários façam logon no YouEarnedIt**, selecione **Logon Único do Azure AD** e clique em **Avançar**.

	![Configurar o logon único](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_03.png)

3. Na página de diálogo **Definir Configurações de Aplicativo**, execute as seguintes etapas:

	![Configurar o logon único](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_04.png)

    a. Na caixa de texto **URL de Logon**, digite a URL usada pelos usuários para fazer logon em seu aplicativo YouEarnedIt usando o seguinte padrão:

	- Ambiente de área restrita: `https://<company name>.sandbox.youearnedit.com/users/sign_in`
	- Ambiente de produção: `https://<company name>.youearnedit.com/users/sign_in`
	
	b. Clique em **Avançar**
 
4. Na página **Configurar logon único no YouEarnedIt**, execute as seguintes etapas:

	![Configurar o logon único](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_05.png)

    a. Clique em **Baixar certificado** e salve o arquivo em seu computador.

    b. Clique em **Próximo**.


5. Para que o SSO seja configurado para seu aplicativo, entre em contato com a equipe de suporte do YouEarnedIt e forneça o seguinte:

	• O **certificado** baixado

	• A **URL de SSO do SAML**

6. No portal clássico, selecione a confirmação da configuração de logon único e clique em **Avançar**.
	
	![Logon único do AD do Azure][10]

7. Na página **Confirmação de logon único**, clique em **Concluir**.
 
	![Logon único do AD do Azure][11]


### Criação de um usuário de teste do AD do Azure
Nesta seção, você criará uma usuária de teste no portal clássico chamada Brenda Fernandes.


![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_09.png)

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_03.png)

4. Para abrir o diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_04.png)

5. Na página do diálogo **Conte-nos sobre este usuário**, execute as seguintes etapas: ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_05.png)

    a. Em Tipo de Usuário, selecione Novo usuário na organização.

    b. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**.

    c. Clique em **Próximo**.

6.  Na página da caixa de diálogo **Perfil do Usuário**, execute as seguintes etapas: ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_06.png)

    a. Na caixa de texto **Nome**, digite **Brenda**.

    b. Na caixa de texto **Sobrenome**, digite **Fernandes**.

    c. Na caixa de texto **Nome de exibição**, digite **Brenda Fernandes**.

    d. Na lista **Função**, selecione **Usuário**.

    e. Clique em **Próximo**.

7. Na página de caixa de diálogo **Obter senha temporária**, clique em **criar**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_07.png)

8. Na página de caixa de diálogo **Obter senha temporária**, execute as seguintes etapas:

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_08.png)

    a. Anote o valor da **Nova Senha**.

    b. Clique em **Concluído**.



### Criação de um usuário de teste do YouEarnedIt

Nesta seção, você criará um usuário chamado Brenda Fernandes no YouEarnedIt. Trabalhe com a equipe de suporte do YouEarnedIt para adicionar os usuários na plataforma do YouEarnedIt.

> [AZURE.NOTE]  O YouEarnedIt espera que o Provedor de Identidade forneça um EmailAddress ou um UserName no atributo NameID. A autenticação falhará se um UserName ou um EmailAddress correspondente não for encontrado no banco de dados ou se não for uma correspondência exata. Isso exigirá que as contas sejam importadas para o sistema do YouEarnedIt antes da integração de SSO (normalmente por meio da importação de API ou CSV).

### Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure, concedendo a ela acesso ao YouEarnedIt.

![Atribuir usuário][200]

**Para atribuir Brenda Fernandes ao YouEarnedIt, execute as seguintes etapas:**

1. No portal clássico, para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **Aplicativos** no menu superior.

	![Atribuir usuário][201]

2. Na lista de aplicativos, selecione **YouEarnedIt**.

	![Configurar o logon único](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_50.png)

3. No menu na parte superior, clique em **Usuários**.

	![Atribuir usuário][203]

4. Na lista de usuários, selecione **Brenda Fernandes**.

5. Na barra de ferramentas na parte inferior, clique em **Atribuir**.

	![Atribuir usuário][205]


### Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco do YouEarnedIt no Painel de Acesso, deverá fazer logon automaticamente no seu aplicativo YouEarnedIt.


## Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0720_2016-->