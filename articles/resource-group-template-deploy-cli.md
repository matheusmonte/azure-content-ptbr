<properties
   pageTitle="Implantar recursos com a CLI do Azure e o modelo | Microsoft Azure"
   description="Use o Azure Resource Manager e a CLI do Azure para implantar recursos no Azure. Os recursos são definidos em um modelo do Resource Manager."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# Implantar recursos com modelos do Resource Manager e a CLI do Azure

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [CLI do Azure](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [API REST](resource-group-template-deploy-rest.md)

Este tópico explica como usar a CLI do Azure com modelos do Resource Manager para implantar seus recursos no Azure.

> [AZURE.TIP] Para obter ajuda com a depuração de erros durante a implantação, consulte:
>
> - [Exibir operações de implantação com a CLI do Azure](resource-manager-troubleshoot-deployments-cli.md) para saber sobre como obter informações que o ajudarão a solucionar o erro
> - [Solucionar erros comuns ao implantar recursos no Azure com o Azure Resource Manager](resource-manager-common-deployment-errors.md) para saber como resolver os erros comuns da implantação

Seu modelo pode ser um arquivo local ou um arquivo externo que está disponível por meio de um URI. Quando seu modelo reside em uma conta de armazenamento, você pode restringir o acesso a ele e fornecer um token de SAS (Assinatura de Acesso Compartilhado) durante a implantação.

## Etapas rápidas para implantação

Este artigo descreve todas as diferentes opções disponíveis para você durante a implantação. No entanto, normalmente você somente precisará de dois comandos simples. Para começar a implantação de forma rápida, use os seguintes comandos:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Para saber mais sobre opções de implantação que podem ser mais adequado para seu cenário, continue lendo este artigo.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## Implantar com a CLI do Azure

Se você não utilizou anteriormente a CLI do Azures com o Gerenciamento de Recursos, consulte [Usando a CLI do Azure para Mac, Linux e Windows e Windows com o Gerenciamento de Recursos do Azure](xplat-cli-azure-resource-manager.md).

1. Faça logon na sua conta do Azure. Depois de fornecer suas credenciais, o comando retornará o resultado do seu logon.

        azure login
  
        ...
        info:    login command OK

2. Se você tiver várias assinaturas, forneça a ID da assinatura que deseja usar para implantação.

        azure account set <YourSubscriptionNameOrId>

3. Alternar para o módulo do Gerenciador de Recursos do Azure. Você receberá a confirmação do novo modo.

        azure config mode arm
   
        info:     New mode is arm

4. Se você não tiver um grupo de recursos existente, crie um grupo de recursos. Forneça o nome do grupo de recursos e o local necessários para sua solução. Você precisa fornecer um local para o grupo de recursos pois o grupo de recursos armazena metadados sobre os recursos. Por motivos de conformidade, talvez você queira especificar onde os metadados são armazenados. Em geral, é recomendável que você especifique um local em que a maioria de seus recursos residirá. Usar o mesmo local pode simplificar seu modelo.

        azure group create -n ExampleResourceGroup -l "West US"

     Um resumo do novo grupo de recursos é retornado.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Valide sua implantação antes de executá-la usando o comando **azure group template validate**. Ao testar a implantação, forneça parâmetros exatamente como faria ao executar a implantação (mostrado na próxima etapa).

        azure group template validate -f <PathToTemplate> -p "{"ParameterName":{"value":"ParameterValue"}}" -g ExampleResourceGroup

5. Para implantar recursos em seu grupo de recursos, execute o comando a seguir e ofereça os parâmetros necessários. Os parâmetros incluem um nome para sua implantação, o nome do seu grupo de recursos, o caminho ou a URL para o modelo e qualquer outro parâmetro necessário para seu cenário.
   
     Você tem as três opções a seguir para fornecer valores de parâmetro:

     1. Use parâmetros embutidos e um modelo local. Cada parâmetro está no formato: `"ParameterName": { "value": "ParameterValue" }`. O exemplo a seguir mostra os parâmetros com caracteres de escape.

            azure group deployment create -f <PathToTemplate> -p "{"ParameterName":{"value":"ParameterValue"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Use parâmetros embutidos e um link para um modelo.

            azure group deployment create --template-uri <LinkToTemplate> -p "{"ParameterName":{"value":"ParameterValue"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Use um arquivo de parâmetro. Para obter informações sobre o arquivo de modelo, consulte [Arquivo de parâmetro](./#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Depois de os recursos serem implantados por meio de um dos três métodos acima, você verá um resumo da implantação.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Para executar uma implantação completa, defina **mode** como **Complete**. Tenha cuidado ao usar esse modo, pois você pode excluir acidentalmente recursos que não estão em seu modelo.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Se você quiser registrar informações adicionais sobre a implantação que podem ajudar a solucionar quaisquer erros de implantação, use o parâmetro **debug-setting**. Você pode especificar que o conteúdo da solicitação, o conteúdo da resposta ou ambos sejam registrados com a operação de implantação.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## Implantar o modelo do armazenamento com um token SAS

Você pode adicionar seus modelos a uma conta de armazenamento e vinculá-los durante a implantação com um token SAS.

> [AZURE.IMPORTANT] Seguindo as etapas abaixo, o blob que contém o modelo fica acessível somente para o proprietário da conta. No entanto, quando você cria um token SAS para o blob, o blob fica acessível para qualquer pessoa com o URI. Se outro usuário interceptar o URI, esse usuário será capaz de acessar o modelo. Usar um token SAS é uma boa maneira de limitar o acesso aos seus modelos, mas você não deve incluir dados confidenciais como senhas diretamente no modelo.

### Adicionar modelo privado à conta de armazenamento

As etapas a seguir configuram uma conta de armazenamento para os modelos:

1. Crie um grupos de recursos.

        azure group create -n "ManageGroup" -l "westus"

2. Criar uma conta de armazenamento. O nome da conta de armazenamento deve ser exclusivo no Azure, então forneça seu próprio nome para a conta.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Definir variáveis para a conta de armazenamento e a chave.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Criar um contêiner. A permissão é definida como **Desativada**, o que significa que o contêiner está acessível apenas para o proprietário.

        azure storage container create --container templates -p Off 
        
4. Adicionar seu modelo ao contêiner.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### Forneça um token SAS durante a implantação

Para implantar um modelo privado em uma conta de armazenamento, recupere um token SAS e inclua-o no URI para o modelo.

1. Crie um token SAS com permissões de leitura e uma hora de vencimento para limitar o acesso. Defina a hora de vencimento de forma a permitir que haja tempo suficiente para concluir a implantação. Recupere o URI completo do modelo, incluindo o token SAS.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Implante o modelo fornecendo o URI que inclui o token SAS.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Para ver um exemplo de como usar um token SAS com modelos vinculados, consulte [Usando modelos vinculados com o Azure Resource Manager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## Próximas etapas
- Para ver um exemplo de como implantar recursos por meio da biblioteca de clientes .NET, consulte [Implantar recursos usando bibliotecas .NET e um modelo](virtual-machines/virtual-machines-windows-csharp-template.md).
- Para definir os parâmetros no modelo, consulte [Criando modelos](resource-group-authoring-templates.md#parameters).
- Para obter orientação sobre como implantar a solução em ambientes diferentes, confira [Ambientes de desenvolvimento e de teste no Microsoft Azure](solution-dev-test-environments.md).
- Para ver os detalhes sobre como usar uma referência do KeyVault para transmitir valores seguros, consulte [Transmitir valores seguros durante a implantação](resource-manager-keyvault-parameter.md).

<!---HONumber=AcomDC_0921_2016-->