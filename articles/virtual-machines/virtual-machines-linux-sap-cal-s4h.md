<properties 
pageTitle="Implantando S/4 HANA ou BW/4 HANA no Microsoft Azure | Microsoft Azure" 
description="Implantando S/4 HANA ou BW/4 HANA no Microsoft Azure" 
services="virtual-machines,virtual-network,storage" 
documentationCenter="saponazure" 
authors="hermanndms" 
manager="juergent" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/>
<tags  
  ms.service="virtual-machines" 
  ms.devlang="NA" 
  ms.topic="campaign-page" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="na" 
  ms.date="09/12/2016" 
  ms.author="hermannd"/>


# Implantando S/4 HANA ou BW/4 HANA no Microsoft Azure 

Este artigo descreve como implantar o S/4 HANA no Microsoft Azure via SAP Cloud Appliance Library 3.0. As capturas de tela mostram o processo passo a passo. Implantar outras soluções baseadas no SAP HANA, como BW/4 HANA, funciona da mesma maneira da perspectiva do processo. Basta selecionar uma solução diferente.

Para iniciar com a SAP Cloud Appliance Library (SAP CAL), acesse [aqui](https://cal.sap.com/). Há um blog do SAP sobre a nova [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).


As seguintes capturas de tela mostram passo a passo como implantar o S/4 HANA no Microsoft Azure. O processo funciona da mesma forma para as outras soluções, como o BW/4 HANA.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

A primeira figura mostra todas as soluções baseadas no SAP CAL HANA disponíveis no Microsoft Azure. Como exemplo, a "edição local do SAP S/4 HANA" (solução na parte inferior na captura de tela) foi escolhida para o processo.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

Primeiro, uma nova conta SAP CAL precisa ser criada. Atualmente, há duas opções para o Azure - Azure padrão e Azure na China continental operado pelo parceiro 21Vianet.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Então, é preciso inserir a ID de assinatura do Azure que pode ser encontrada no portal do Azure - veja também mais abaixo como obtê-la. Depois, um certificado de gerenciamento do Azure precisa ser baixado.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

No novo portal do Azure, localiza-se o item "Assinaturas" no lado esquerdo. Clique para mostrar todas as assinaturas ativas para o usuário.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Selecionar uma das assinaturas e escolher "Certificados de Gerenciamento" explica que existe um novo conceito usando as "entidades de serviço" para o novo modelo do Azure Resource Manager. O CAL SAP não está adaptado ainda para esse novo modelo e ainda requer o modelo "clássico" e o antigo portal do Azure para trabalhar com os certificados de gerenciamento.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Aqui, é possível ver o antigo portal do Azure. O carregamento de um certificado de gerenciamento fornece ao SAP CAL permissões para criar máquinas virtuais dentro de uma assinatura do cliente. Na guia "ASSINATURAS", é possível encontrar a ID da assinatura a ser inserida no portal do SAP CAL.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Na segunda guia, é possível carregar o certificado de gerenciamento que foi baixado anteriormente no SAP CAL.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Uma pequena caixa de diálogo é exibida para selecionar o arquivo do certificado baixado.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Depois do certificado ser carregado, a conexão entre o SAP CAL e a assinatura do Azure do cliente pode ser testada no SAP CAl. Uma pequena mensagem deve ser exibida informando que a conexão é válida.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Após a configuração de uma conta, é preciso selecionar uma solução que deve ser implantada e criar uma instância. Com o modo "básico", é realmente simples. Insira um nome de instância, escolha uma região do Azure e defina a senha principal para a solução.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Após algum tempo, dependendo do tamanho e da complexidade da solução (uma estimativa é fornecida pelo SAP CAL), aparece como "ativo" e pronto para o uso. É muito simples.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Examinando alguns detalhes da solução, é possível ver qual tipo de VM foi implantada. Nesse caso, três VMs do Azure de diferentes tamanhos e finalidade foram criadas.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

No portal do Azure, as máquinas virtuais podem ser encontradas começando com o mesmo nome de instância fornecido no SAP CAL.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Agora, é possível conectar a solução por meio do botão Conectar no portal do SAP CAL. A pequena caixa de diálogo contém um link para um guia do usuário que descreve todas as credenciais padrão para trabalhar com a solução.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Uma opção é fazer logon na VM Windows do cliente e iniciar, por exemplo, a GUI do SAP pré-configurada.

<!---HONumber=AcomDC_0914_2016-->