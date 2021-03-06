<properties 
   pageTitle="Criar um balanceador de carga voltado para a Internet no Gerenciador de Recursos usando a CLI do Azure | Microsoft Azure"
   description="Saiba como criar um balanceador de carga voltado para a Internet no Gerenciador de Recursos usando a CLI do Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# Criar um balanceador de carga interno usando a CLI do Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] Este artigo aborda o modelo de implantação do Gerenciador de Recursos. Também é possível [Saber como criar um balanceador de carga para a Internet usando a implantação clássica](load-balancer-get-started-internet-classic-portal.md).


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## Implantar a solução usando a CLI do Azure

As etapas a seguir mostram como criar um balanceador de carga para a Internet usando o Azure Resource Manager com a CLI. Com o Azure Resource Manager, todos os recursos são criados e configurados individualmente e, em seguida, colocados juntos para criar um recurso.

Você deve criar e configurar os seguintes objetos para implantar um balanceador de carga:

- Configuração de IP de front-end – contém endereços IP públicos para o tráfego de rede de entrada.
- Pool de endereços de back-end – contém NICs (interfaces de rede) para que as máquinas virtuais recebam o tráfego de rede do balanceador de carga.
- Regras de balanceamento de carga – contém regras de mapeamento de uma porta pública no balanceador de carga para uma porta no pool de endereços de back-end.
- Regras NAT de entrada – contém regras de mapeamento de uma porta pública no balanceador de carga para uma porta de uma máquina virtual específica no pool de endereços de back-end.
- Investigações – contém investigações de integridade usadas para verificar a disponibilidade de instâncias de máquinas virtuais no pool de endereços de back-end.

Para obter mais informações, confira [Suporte do Azure Resource Manager para balanceador de carga](load-balancer-arm.md).

## Configurar a CLI para usar o Gerenciador de Recursos

1. Se você nunca usou a CLI do Azure, consulte [Instalar e configurar a CLI do Azure](../../articles/xplat-cli-install.md) e siga as instruções até o ponto em que você seleciona sua conta e assinatura do Azure.

2. Execute o comando **azure config mode** para alternar para o modo do Gerenciador de Recursos, como mostrado abaixo.

        azure config mode arm

    Saída esperada:

        info:    New mode is arm

## Criar uma rede virtual e um endereço IP público para o pool de IPs de front-end

1. Crie uma rede virtual (VNet) denominada *NRPVnet* no local Leste dos EUA usando um grupo de recursos denominado *NRPRG*.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Crie uma sub-rede denominada *NRPVnetSubnet* com um bloco CIDR de 10.0.0.0/24 na *NRPVnet*.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Crie um endereço IP público denominado *NRPPublicIP* para ser usado por um pool de IPs front-end com nome DNS *loadbalancernrp.eastus.cloudapp.azure.com*. O comando a seguir usa o tipo de alocação estática e o tempo limite de ociosidade de 4 minutos.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT] O balanceador de carga usará o rótulo de domínio do IP público como FQDN. Isso é uma mudança da implantação clássica, que usa o serviço de nuvem como o FQDN do balanceador de carga. Neste exemplo, o FQDN é *loadbalancernrp.eastus.cloudapp.azure.com*.

## Criar um balanceador de carga

O comando a seguir cria um balanceador de carga chamado *NRPlb* no grupo de recursos *NRPRG*, na localização do Azure *Leste dos EUA*.

    azure network lb create NRPRG NRPlb eastus

## Criar um pool de IPs de front-end e um pool de endereços de back-end

Este exemplo cria o pool de IPs de front-end, que recebe o tráfego de rede de entrada no balanceador de carga, e o pool de IPs de back-end, no qual o pool de front-end envia o tráfego de rede com balanceamento de carga.

1. Crie um pool de IPs de front-end associando o IP público criado na etapa anterior e o balanceador de carga.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Configure um pool de endereços de back-end usado para receber o tráfego de entrada do pool de IPs de front-end.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## Criar regras de balanceamento de carga, regras NAT e investigação

Este exemplo cria os seguintes itens.

- uma regra NAT para converter todo o tráfego de entrada na porta 21 para a porta 22<sup>1</sup>
- uma regra NAT para converter todo o tráfego de entrada na porta 23 para a porta 22
- uma regra do balanceador de carga para equilibrar todo o tráfego de entrada na porta 80 para a porta 80 nos endereços no pool de back-end.
- uma regra de investigação para verificar o status de integridade em uma página chamada *HealthProbe.aspx*.

<sup>1</sup> As regras NAT são associadas a uma instância de máquina virtual específica por trás do balanceador de carga. O tráfego de rede que chega à porta 21 é enviado a uma máquina virtual específica na porta 22 associada a esta regra NAT. Você deve especificar um protocolo (UDP ou TCP) para uma regra NAT. Ambos os protocolos não podem ser atribuídos à mesma porta.

1. Criar regras NAT.

        azure network lb inbound-nat-rule create -g nrprg -l nrplb -n ssh1 -p tcp -f 21 -b 22
        azure network lb inbound-nat-rule create -g nrprg -l nrplb -n ssh2 -p tcp -f 23 -b 22

    Parâmetros:
    * **-g** – nome do grupo de recursos
    * **-l** – nome do balanceador de carga
    * **- n** – nome do recurso, quer seja a regra NAT, a regra de investigação ou de balanceamento de carga.
    * **-p** – protocolo (pode ser TCP ou UDP)
    * **-f** – porta de front-end a ser usada (o comando Investigação usa -f para definir o caminho da investigação)
    * **-b** – porta de back-end a ser usada

2. Crie uma regra de balanceador de carga.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Crie um teste de integridade.

        azure network lb probe create -g nrprg -l nrplb -n healthprobe -p "http" -o 80 -f healthprobe.aspx -i 15 -c 4

    Parâmetros:
    * **-g** – grupo de recursos
    * **-l** – nome do conjunto do balanceador de carga
    * **- n** – nome da investigação de integridade
    * **-p** – protocolo usado pela investigação de integridade
    * **-i** – intervalo de investigação em segundos
    * **- c** – número de verificações

4. Verifique suas configurações.

        azure network lb show nrprg nrplb

    Saída esperada:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## Criar NICs

Você precisa criar NICs (ou modificar as existentes) e associá-las a regras NAT, regras do balanceador de carga e testes.

1. Crie uma NIC chamada *lb-nic1-be* e a associe à regra NAT *rdp1* e ao pool de endereços de back-end *NRPbackendpool*.

        azure network nic create -g nrprg -n lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet -d "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" -e "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Parâmetros:

    * **-g** - nome do grupo de recursos
    * **-n** - nome do recurso NIC
    * **--subnet-name** - nome da sub-rede
    * **--subnet-vnet-name** - nome da rede virtual
    * **-d** – a ID do recurso de pool de back-end – começa com /subscription/{subscriptionID/resourcegroups/<nome do grupo de recursos>/providers/Microsoft.Network/loadbalancers/<nome do balanceador de carga>/backendaddresspools/<nome do pool de back-end>
    * **-e** – a ID da regra NAT deve ser associada ao recurso NIC – começa com /subscriptions/####################################/resourceGroups/<nome do grupo de recursos>/providers/Microsoft.Network/loadBalancers/<nome do balanceador de carga>/inboundNatRules/<nome da regra NAT>

    Saída esperada:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Crie uma NIC chamada *lb-nic2-be* e a associe à regra NAT *rdp2* e ao pool de endereços de back-end *NRPbackendpool*.

        azure network nic create -g nrprg -n lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet -d "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" -e "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Crie uma máquina virtual (VM) chamada *web1* e associá-la ao NIC chamado *lb-nic1-be*. Uma conta de armazenamento denominada *web1nrp* foi criada antes da execução do comando a seguir.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] As VMs em um balanceador de carga precisam estar no mesmo conjunto de disponibilidade. Use `azure availset create` para criar um conjunto de disponibilidade.

    A saída deverá ser semelhante a esta:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] A mensagem informativa **Esta é uma NIC sem IP público configurado** é esperada, pois a NIC criada para o balanceador de carga se conecta à Internet usando o endereço IP público do balanceador de carga.

    Como a NIC *lb-nic1-be* está associada à regra NAT *rdp1*, você pode se conectar ao *web1* usando o RDP por meio da porta 3441 no balanceador de carga.

4. Crie uma máquina virtual (VM) denominada *web2* e a associe à NIC denominada *lb-nic2-be*. Uma conta de armazenamento chamada *web1nrp* foi criada antes da execução do comando a seguir.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## Atualizar um balanceador de carga existente

Você pode adicionar regras ao fazer referência a um balanceador de carga existente. No exemplo a seguir, uma nova regra de balanceador de carga é adicionada a um balanceador de carga **NRPlb** existente

    azure network lb rule create -g nrprg -l nrplb -n lbrule2 -p tcp -f 8080 -b 8051 -t frontendnrppool -o NRPbackendpool

Parâmetros:

* **-g** – nome do grupo de recursos
* **-l** – nome do balanceador de carga
* **- n** – nome da regra do balanceador de carga
* **-p** – protocolo
* **-f** – porta de front-end
* **-b** – porta de back-end
* **-t** – nome do pool de front-end
* **-b** – nome do pool de back-end

## Excluir um balanceador de carga

Para remover um balanceador de carga, use o comando a seguir:

    azure network lb delete -g nrprg -n nrplb

Em que **nrprg** é o grupo de recursos e **nrplb** é o nome do balanceador de carga.

## Próximas etapas

[Introdução à configuração de um balanceador de carga interno](load-balancer-get-started-ilb-arm-cli.md)

[Configurar um modo de distribuição do balanceador de carga](load-balancer-distribution-mode.md)

[Definir configurações de tempo limite de TCP ocioso para o balanceador de carga](load-balancer-tcp-idle-timeout.md)

<!---HONumber=AcomDC_0928_2016-->