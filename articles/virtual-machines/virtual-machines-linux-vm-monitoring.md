<properties
   pageTitle="Habilitar ou desabilitar o monitoramento da VM do Azure"
   description="Descreve como habilitar ou desabilitar o monitoramento da VM do Azure"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# Habilitar ou desabilitar o monitoramento da VM do Azure

Esta seção descreve como habilitar ou desabilitar o monitoramento de máquinas virtuais em execução no Azure. Por padrão, o monitoramento é habilitado nas máquinas virtuais do Azure se implantadas pelo [portal do Azure](https://portal.azure.com) e os gráficos de monitoramento são fornecidos por padrão com um período de um minuto. Você pode habilitar ou desabilitar o monitoramento usando o portal ou a interface de linha de comando do Azure para Mac, Linux e Windows (a CLI do Azure).

## Habilitar/desabilitar o monitoramento por meio do portal do Azure
 
É possível habilitar o monitoramento da VM do Azure, que fornece dados sobre a sua instância em períodos de um minuto (aplicam-se alterações de armazenamento). Os dados detalhados de diagnóstico são então disponibilizados para a VM nos gráficos de portal ou por meio da API. Por padrão, o portal do Azure permite o monitoramento, mas você pode desativá-lo, conforme descrito abaixo. Você pode habilitar o monitoramento enquanto a VM estiver em execução ou no estado parado.

- Abra o portal do Azure em **[https://portal.azure.com](https://portal.azure.com)**

- Na navegação à esquerda, clique em Máquinas virtuais.

- Na lista de máquinas virtuais, escolha uma instância em execução ou parada. A folha Máquina virtual será aberta.

- Clique em "Todas as configurações".

- Clique em "Diagnóstico".

- Altere o status para Ativado ou Desativado. Nessa folha, você também pode escolher o nível de detalhes de monitoramento que deseja habilitar para a máquina virtual.

[Azure.Note] A opção Diagnóstico Ativado é o padrão quando você cria uma nova máquina virtual

![Habilite/desabilite o monitoramento por meio do Portal do Azure.][1]


## Habilitar/desabilitar o monitoramento com a CLI do Azure
 
Para habilitar o monitoramento de uma VM do Azure.

- Crie um arquivo denominado PrivateConfig.json com o conteúdo a seguir. { "storageAccountName":"a conta de armazenamento para receber dados", "storageAccountKey":"a chave da conta" }
- Execute o comando da CLI do Azure a seguir.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] É possível alterar da versão 2.0 para uma versão posterior quando disponível.

Para obter mais detalhes sobre como configurar as métricas de monitoramento e exemplos, acesse o documento que descreve **[como usar a extensão de diagnóstico do Linux para monitorar dados de desempenho e diagnóstico da VM Linux](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

<!---HONumber=AcomDC_0824_2016-->