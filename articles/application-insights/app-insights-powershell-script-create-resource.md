<properties 
	pageTitle="Script do PowerShell para criar um recurso do Application Insights" 
	description="Automatize a criação de recursos do Application Insights." 
	services="application-insights" 
    documentationCenter="windows"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/19/2016" 
	ms.author="awills"/>

#  Script do PowerShell para criar um recurso do Application Insights

*O Application Insights está em modo de visualização.*

Quando você deseja monitorar um novo aplicativo, ou uma nova versão de um aplicativo, com o [Visual Studio Application Insights](https://azure.microsoft.com/services/application-insights/), você configura um novo recurso no Microsoft Azure. Este recurso é o local no qual os dados de telemetria do seu aplicativo são analisados e exibidos.

Você pode automatizar a criação de um novo recurso usando o PowerShell.

Por exemplo, se você estiver desenvolvendo um aplicativo de dispositivo móvel, é provável que, a qualquer momento, haverá várias versões publicadas do seu aplicativo em uso por seus clientes. Não é recomendável obter os resultados da telemetria de diferentes versões misturadas. Por isso, obtenha seu processo de compilação para criar um novo recurso para cada compilação.

## Script para criar um recurso do Application Insights

Consulte as especificações do cmdlet relevante:

* [New-AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*Script do PowerShell*

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## O que fazer com o iKey

Cada recurso é identificado por sua chave de instrumentação (iKey). O iKey é uma saída do script de criação de recursos. Seu script de compilação deve fornecer o iKey para o SDK do Application Insights inserido no seu aplicativo.

Há duas maneiras de disponibilizar o iKey para o SDK:
  
* No [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Ou no [código de inicialização](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## Consulte também

* [Criar recursos de teste da Web e do Application Insights por meio de modelos](app-insights-powershell.md)
* [Configurar o monitoramento do diagnóstico do Azure com o PowerShell](app-insights-powershell-azure-diagnostics.md) 
* [Definir alertas usando o PowerShell](app-insights-powershell-alerts.md)

 

<!---HONumber=AcomDC_0224_2016-->