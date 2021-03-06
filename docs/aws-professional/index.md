---
title: Azure para profissionais do AWS
titleSuffix: Azure Architecture Center
description: Entenda os conceitos básicos de contas, plataforma e serviços do Microsoft Azure. Conheça também as principais semelhanças e diferenças entre as plataformas AWS e Azure. Aproveite sua experiência com o AWS no Azure.
keywords: Especialistas em AWS, comparação com o Azure, comparação com o AWS, diferença entre azure e aws, azure e aws
author: lbrader
ms.date: 09/19/2018
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 56e308b40e24d2febefe995dffc7a14069a5c078
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54482231"
---
# <a name="azure-for-aws-professionals"></a>Azure para profissionais do AWS

Este artigo ajuda os especialistas em AWS (Amazon Web Services) a compreender os conceitos básicos de contas, plataforma e serviços do Microsoft Azure. Aborda também as principais semelhanças e diferenças entre as plataformas AWS e Azure.

O que você aprenderá:

- Como contas e recursos são organizados no Azure.
- Como as soluções disponíveis são estruturadas no Azure.
- As principais diferenças entre os serviços Azure e AWS.

O Azure e o AWS construíram seus recursos independentemente, ao longo do tempo, e têm diferenças importantes de design e implementação.

## <a name="overview"></a>Visão geral

Tal como o AWS, o Microsoft Azure foi criado para prestar um conjunto central de serviços de computação, armazenamento, banco de dados e rede. Em muitos casos, ambas as plataformas têm uma equivalência básica entre os produtos e serviços que oferecem. O AWS e o Azure permitem criar soluções altamente disponíveis com base em hosts do Windows ou do Linux. Portanto, se você estiver acostumado ao desenvolvimento com tecnologia OSS e Linux, as duas plataformas são eficazes.

Embora elas tenham funcionalidades semelhantes, os recursos que as fornecem são, em geral, organizados de forma diferente. As relações um-para-um entre os serviços necessários para compilar uma solução nem sempre são claras. Também há casos em que um serviço específico pode ser oferecido em uma plataforma, mas não na outra. Confira os [gráficos comparativos de serviços do Azure e do AWS](./services.md).

## <a name="accounts-and-subscriptions"></a>Contas e assinaturas

Os serviços do Azure podem ser adquiridos em várias opções de preços, dependendo do tamanho e das necessidades da sua organização. Confira os detalhes na página de [visão geral de preços](https://azure.microsoft.com/pricing/).

As [assinaturas do Azure](/azure/virtual-machines/linux/infrastructure-example) são um agrupamento de recursos com um proprietário atribuído, responsável por gerenciar cobranças e permissões. Diferentemente do AWS, onde todos os recursos criados sob a conta AWS são vinculados a essa conta, as assinaturas existem independentemente das contas do proprietário e podem ser reatribuídas a novos proprietários conforme necessário.

<!-- markdownlint-disable MD033 -->

![Comparação entre a estrutura e a propriedade de contas do AWS e assinaturas do Azure](./images/azure-aws-account-compare.png "Comparação entre a estrutura e a propriedade de contas do AWS e assinaturas do Azure")
<br/>*Comparação entre a estrutura e a propriedade de contas do AWS e assinaturas do Azure*
<br/><br/>

<!-- markdownlint-enable MD033 -->

As assinaturas são atribuídas a três tipos de contas de administrador:

- **Administrador da Conta**. É o proprietário da assinatura, e também a conta responsável pelo pagamento dos recursos usados na assinatura. O administrador da conta só pode ser alterado transferindo-se a propriedade da assinatura.

- **Administrador de Serviços**. Esta conta tem direitos para criar e gerenciar recursos na assinatura, mas não responde pelas questões de cobrança. Por padrão, o administrador da conta e o administrador de serviços são atribuídos à mesma conta. O administrador da conta pode atribuir um usuário separado à conta de administrador de serviços para gerenciar os aspectos técnicos e operacionais de uma assinatura. Há somente um administrador de serviços por assinatura.

- **Coadministrador**. Pode haver várias contas de coadministrador atribuídas a uma assinatura. Os coadministradores não podem mudar o administrador de serviços, mas, quanto ao mais, têm controle total sobre os recursos de assinatura e os usuários.

Abaixo do nível de assinatura, as permissões de funções de usuário e individuais também podem ser atribuídas a recursos específicos, tal como ocorre com a concessão de permissões a usuários e grupos do IAM no AWS. No Azure, todas as contas de usuário são associadas a uma Conta da Microsoft ou Conta Organizacional (gerenciada por meio de um Active Directory do Azure).

Tal como as contas do AWS, as assinaturas têm cotas e limites de serviço padrão. Para obter uma lista completa desses limites, confira [Limites, cotas e restrições em assinaturas e serviços do Azure](/azure/azure-subscription-service-limits).
Esses limites podem ser aumentados até o máximo [preenchendo-se uma solicitação de suporte no portal de gerenciamento](https://blogs.msdn.microsoft.com/girishp/2015/09/20/increasing-core-quota-limits-in-azure/).

### <a name="see-also"></a>Consulte também

- [Como adicionar ou alterar funções de administrador do Azure](/azure/billing/billing-add-change-azure-subscription-administrator)

- [Como baixar sua fatura de cobrança e dados de uso diário do Azure](/azure/billing/billing-download-azure-invoice-daily-usage-date)

## <a name="resource-management"></a>Gerenciamento de recursos

O termo "recurso" no Azure é usado tal como no AWS, significando qualquer instância de computação, objeto de armazenamento, dispositivo de rede ou outra entidade que você pode criar ou configurar na plataforma.

Os recursos do Azure são implantados e gerenciados usando um destes dois modelos: [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) ou o [modelo de implantação clássico](/azure/azure-resource-manager/resource-manager-deployment-model) do Azure mais antigo. Todos os recursos novos são criados com o modelo Resource Manager.

### <a name="resource-groups"></a>Grupos de recursos

O Azure e o AWS têm entidades chamadas "grupos de recursos", que organizam recursos como VMs, armazenamento e dispositivos de rede virtuais. No entanto, os [grupos de recursos do Azure](/azure/virtual-machines/windows/infrastructure-example) não se comparam diretamente aos do AWS.

O AWS permite que um recurso seja marcado em vários grupos de recursos; já no Azure, ele está sempre associado um grupo de recursos. Um recurso criado em um grupo de recursos pode ser movido para outro, mas só pode estar em um por vez. Os grupos de recursos são o agrupamento fundamental usado pelo Azure Resource Manager.

Os recursos também podem ser organizados usando-se [marcas](/azure/azure-resource-manager/resource-group-using-tags). Marcas são pares chave-valor que permitem agrupar recursos na assinatura, independentemente da associação ao grupo de recursos.

### <a name="management-interfaces"></a>Interfaces de gerenciamento

O Azure oferece várias maneiras de gerenciar seus recursos:

- [Interface da Web](/azure/azure-resource-manager/resource-group-portal).
    Como o painel do AWS, o portal do Azure fornece uma interface totalmente baseada na Web para o gerenciamento dos recursos do Azure.

- [API REST](/rest/api/).
    A REST API do Azure Resource Manager fornece acesso programático à maioria dos recursos disponíveis no portal do Azure.

- [Linha de Comando](/azure/azure-resource-manager/cli-azure-resource-manager).
    A ferramenta Azure CLI 2.0 fornece uma interface de linha de comando capaz de criar e gerenciar recursos do Azure. A CLI do Azure está disponível para [Windows, Linux e Mac OS](https://aka.ms/azurecli2).

- [PowerShell](/azure/azure-resource-manager/powershell-azure-resource-manager).
    Os módulos do Azure para PowerShell permitem executar tarefas de gerenciamento automatizado usando um script. O PowerShell está disponível para [Windows, Linux e Mac OS](https://github.com/PowerShell/PowerShell).

- [Modelos](/azure/azure-resource-manager/resource-group-authoring-templates).
    Os modelos do Azure Resource Manager fornecem recursos JSON de gerenciamento baseado em modelo semelhantes aos do serviço CloudFormation do AWS.

Em cada uma dessas interfaces, o grupo de recursos é fundamental para o modo como os recursos do Azure são criados, implantados ou modificados. Isso é semelhante à função da "pilha" no agrupamento de recursos do AWS durante as implantações de CloudFormation.

A sintaxe e a estrutura dessas interfaces diferem das de seus equivalentes do AWS, mas os recursos são comparáveis. Além disso, muitas ferramentas de gerenciamento de terceiros usadas no AWS, como [Terraform do Hashicorp](https://www.terraform.io/docs/providers/azurerm/) e [Netflix Spinnaker](https://www.spinnaker.io/), também estão disponíveis no Azure.

<!-- markdownlint-disable MD024 -->

### <a name="see-also"></a>Consulte também

- [Diretrizes de grupos de recursos do Azure](/azure/azure-resource-manager/resource-group-overview#resource-groups)

## <a name="regions-and-zones-high-availability"></a>Regiões e zonas (alta disponibilidade)

Falhas podem variar quanto ao escopo do seu impacto. Algumas falhas de hardware, como um disco com falha, podem afetar um único computador host. Um comutador de rede com falha pode afetar um rack de servidor inteiro. Menos comuns são falhas que interrompem um data center inteiro, tais como uma perda de energia em um data center. Raramente, toda uma região pode se tornar não disponível.

Uma das principais maneiras de se tornar um aplicativo resiliente é por meio de redundância. Mas você precisa planejar essa redundância ao projetar o aplicativo. Além disso, o nível de redundância necessário depende de suas necessidades de negócios &mdash; nem todo aplicativo precisa de redundância entre regiões para se proteger contra uma interrupção regional. Em geral, há um equilíbrio entre mais redundância e confiabilidade em relação a maiores custo e complexidade.

No AWS, uma região é dividida em duas ou mais Zonas de Disponibilidade. Uma Zona de Disponibilidade corresponde a um datacenter fisicamente isolado na região geográfica. O Azure tem vários recursos para tornar um aplicativo redundante em cada nível de falha, incluindo **conjuntos de disponibilidade**, **zonas de disponibilidade** e **zonas emparelhadas**.

![Redundância](../resiliency/images/redundancy.svg)

A tabela a seguir resume cada opção.

| &nbsp; | Conjunto de disponibilidade | Zona de disponibilidade | Região emparelhada |
|--------|------------------|-------------------|---------------|
| Escopo da falha | Rack | Datacenter | Região |
| Roteamento de solicitação | Load Balancer | Load Balancer entre zonas | Gerenciador de Tráfego |
| Latência da rede | Muito baixa | Baixo | Média a alta |
| Rede Virtual  | VNET | VNET | Emparelhamento VNET entre regiões |

### <a name="availability-sets"></a>Conjuntos de disponibilidade

Para se proteger contra falhas de hardware localizadas, como falha em um comutador de rede ou de disco, implante duas ou mais VMs em um conjunto de disponibilidade. Um conjunto de disponibilidade consiste em dois ou mais *domínios de falha* que compartilham um comutador de rede e uma fonte de energia comuns. Máquinas virtuais em um conjunto de disponibilidade são distribuídas entre os domínios de falha, portanto, se uma falha de hardware afeta um domínio de falha, o tráfego de rede ainda poderá ser roteado para as VMs nos outros domínios de falha. Para obter mais informações sobre conjuntos de disponibilidade, consulte [Gerenciar a disponibilidade das máquinas virtuais do Windows no Azure](/azure/virtual-machines/windows/manage-availability).

Quando instâncias de VM são adicionadas a conjuntos de disponibilidade, elas também recebem um [domínio de atualização](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-manage-availability/). Um domínio de atualização é um grupo de VMs definidas para eventos de manutenção planejada ao mesmo tempo. A distribuição de VMs em vários domínios de atualização garante que os eventos de atualização planejada e de aplicação de patch afetem apenas um subconjunto dessas VMs em determinado momento.

Os conjuntos de disponibilidade devem ser organizados pela função da instância no seu aplicativo, para garantir que uma instância de cada função esteja operacional. Por exemplo, em um aplicativo web de três camadas, crie conjuntos de disponibilidade separados para as camadas de front-end, aplicativos e camadas de dados.

![Conjuntos de disponibilidade do Azure para cada função de aplicativo](./images/three-tier-example.png "Conjuntos de disponibilidade para cada função de aplicativo")

### <a name="availability-zones"></a>Zonas de disponibilidade

Uma [Zona de Disponibilidade](/azure/availability-zones/az-overview) é uma zona fisicamente separada em uma região do Azure. Cada zona de disponibilidade tem uma rede, resfriamento e fonte de energia distintos. A implantação de VMs em zonas de disponibilidade ajuda a proteger um aplicativo contra falhas em todo o datacenter.

### <a name="paired-regions"></a>Regiões emparelhadas

Para proteger um aplicativo contra uma interrupção regional, você pode implantar o aplicativo em várias regiões, usando o [Gerenciador de Tráfego do Azure][traffic-manager] para distribuir o tráfego de Internet para as diferentes regiões. Cada região do Azure é emparelhada com outra. Juntas, elas formam um [par regional][paired-regions]. Com a exceção do Sul do Brasil, pares regionais estão localizados na mesma região geográfica para atender aos requisitos de residência de dados para fins de jurisdição de imposição fiscal e legal.

Diferentemente das Zonas de Disponibilidade, que, embora sejam datacenters fisicamente separados, podem estar em áreas geográficas relativamente próximas, as regiões emparelhadas ficam, em geral, distantes no mínimo 300 milhas (cerca de 480 km). Isso visa a garantir que desastres em maior escala afetem somente uma das regiões do par. Os pares vizinhos podem ser definidos para sincronizar dados de serviços de armazenamento e de bancos de dados, e são configurados para que as atualizações de plataforma sejam distribuídas em apenas uma região do par de cada vez.

O backup do [armazenamento com redundância geográfica](/azure/storage/common/storage-redundancy-grs) do Azure é feito automaticamente na região emparelhada apropriada. Em todos os outros recursos, criar uma solução totalmente redundante usando regiões emparelhadas significa gerar uma cópia completa da sua solução em ambas as regiões.

### <a name="see-also"></a>Consulte também

- [Regiões e disponibilidade para máquinas virtuais no Azure](/azure/virtual-machines/linux/regions-and-availability)

- [Alta disponibilidade para os aplicativos do Azure](../resiliency/high-availability-azure-applications.md)

- [Recuperação de desastre para aplicativos do Azure](../resiliency/disaster-recovery-azure-applications.md)

- [Manutenção planejada para máquinas virtuais Linux no Azure](/azure/virtual-machines/linux/maintenance-and-updates)

## <a name="services"></a>Serviços

Para obter uma lista como os serviços são mapeados entre plataformas, confira [comparação dos serviços do AWS para o Azure](./services.md).

Nem todos os produtos e serviços do Azure estão disponíveis em todas as regiões. Confira a página [Produtos por região](https://azure.microsoft.com/global-infrastructure/services/) para obter detalhes. Você pode encontrar as garantias de tempo de atividade e políticas de crédito de tempo de inatividade para cada produto ou serviço do Azure na página [Contratos de nível de serviço](https://azure.microsoft.com/support/legal/sla/).

As seções a seguir fornecem uma breve explicação das diferenças entre os serviços e recursos comumente usados nas plataformas AWS e Azure.

### <a name="compute-services"></a>Serviços de computação

#### <a name="ec2-instances-and-azure-virtual-machines"></a>Instâncias do EC2 e máquinas virtuais do Azure

Embora os tipos de instâncias do AWS e os tamanhos de máquinas virtuais do Azure se dividam de maneira semelhante, há diferenças nos recursos de RAM, CPU e armazenamento.

- [Tipos de instâncias do Amazon EC2](https://aws.amazon.com/ec2/instance-types/)

- [Tamanhos das máquinas virtuais no Azure (Windows)](/azure/virtual-machines/windows/sizes)

- [Tamanhos das máquinas virtuais no Azure (Linux)](/azure/virtual-machines/linux/sizes)

Semelhante à cobrança por segundo do AWS, as VMs sob demanda do Azure são cobradas por minuto.

#### <a name="ebs-and-azure-storage-for-vm-disks"></a>EBS e Armazenamento do Azure para discos de VM

O armazenamento de dados durável para VMs do Azure é fornecido por [discos de dados](/azure/virtual-machines/linux/about-disks-and-vhds) que residem no armazenamento de blobs. Isso é semelhante à forma como as instâncias do EC2 armazenam volumes de disco em Elastic Block Store (EBS). O [Armazenamento temporário do Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) também fornece às VMs o mesmo armazenamento temporário de leitura-gravação com baixa latência do Instance Storage do EC2 (também chamado de armazenamento efêmero).

A E/S em disco de desempenho superior é suportada com o [armazenamento premium do Azure](/azure/virtual-machines/windows/premium-storage). Isso é semelhante às opções de armazenamento provisionado IOPS fornecidas pelo AWS.

#### <a name="lambda-azure-functions-azure-web-jobs-and-azure-logic-apps"></a>Lambda, Azure Functions, Azure Web-Jobs e Aplicativos Lógicos do Azure

O [Azure Functions](https://azure.microsoft.com/services/functions/) é o equivalente principal do Lambda do AWS no fornecimento de código sem servidor sob demanda. No entanto, a funcionalidade do Lambda também se sobrepõe a outros serviços do Azure:

- Os [WebJobs](/azure/app-service/web-sites-create-web-jobs) permitem criar tarefas em segundo plano agendadas ou de execução contínua.

- Os [Aplicativos Lógicos](https://azure.microsoft.com/services/logic-apps/) fornecem serviços de comunicação, integração e gerenciamento de regras de negócios.

#### <a name="autoscaling-azure-vm-scaling-and-azure-app-service-autoscale"></a>Dimensionamento automático, escala de VM do Azure e Dimensionamento Automático de Serviço de Aplicativo do Azure

O dimensionamento automático no Azure é tratado por dois serviços:

- Os [Conjuntos de Dimensionamento de VM](/azure/virtual-machine-scale-sets/overview) permitem implantar e gerenciar um conjunto idêntico de VMs. O número de instâncias pode ser dimensionado automaticamente com base nas necessidades de desempenho.

- O [Dimensionamento Automático do Serviço de Aplicativo](/azure/app-service/web-sites-scale) permite o dimensionamento automático de soluções do Serviço de Aplicativo do Azure.

#### <a name="container-service"></a>Serviço de Contêiner

O [Serviço de Kubernetes do Azure](/azure/aks/intro-kubernetes) oferece suporte a contêineres do Docker gerenciados pelo Kubernetes.

#### <a name="other-compute-services"></a>Outros serviços de computação

O Azure oferece vários serviços de computação sem equivalente direto no AWS:

- O [Lote do Azure](/azure/batch/batch-technical-overview) permite gerenciar trabalhos de computação intensiva em um conjunto dimensionável de máquinas virtuais.

- O [Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-overview) é uma plataforma para desenvolvimento e hospedagem de soluções escalonáveis de [microsserviços](/azure/service-fabric/service-fabric-overview-microservices).

#### <a name="see-also"></a>Consulte também

- [Criar uma VM do Linux no Azure usando o portal](/azure/virtual-machines/linux/quick-create-portal)

- [Arquitetura de Referência do Azure: Executando uma VM do Linux no Azure](/azure/architecture/reference-architectures/n-tier/linux-vm)

- [Get started with Node.js web apps in Azure App Service (Introdução aos aplicativos Web do Node.js no Serviço de Aplicativo do Azure)](/azure/app-service/app-service-web-get-started-nodejs)

- [Arquitetura de Referência do Azure: aplicativo Web básico](/azure/architecture/reference-architectures/app-service-web-app/basic-web-app)

- [Criar sua primeira Função do Azure](/azure/azure-functions/functions-create-first-azure-function)

### <a name="storage"></a>Armazenamento

#### <a name="s3ebsefs-and-azure-storage"></a>S3/EBS/EFS e Armazenamento do Azure

Na plataforma AWS, o armazenamento em nuvem divide-se principalmente em três serviços:

- **Serviços de Armazenamento Simples (S3)**. O armazenamento de objeto básico que disponibiliza os dados por meio de uma API acessível da Internet.

- **Elastic Block Store (EBS)**. Armazenamento em nível de bloco destinado ao acesso por uma única VM.

- **Elastic File System (EFS)**. Armazenamento de arquivos para uso compartilhado, comportando milhares de instâncias de EC2.

No Armazenamento do Azure, as [contas de armazenamento](/azure/storage/common/storage-quickstart-create-account) associadas à assinatura permitem criar e gerenciar os serviços de armazenamento a seguir:

- O [armazenamento de blobs](/azure/storage/common/storage-quickstart-create-account) armazena qualquer tipo de texto ou de dados binários, como documentos, arquivos de mídia ou instaladores de aplicativos. Você pode definir o armazenamento de Blobs para acesso privado ou para compartilhar conteúdo público na Internet. O armazenamento de blobs tem a mesma finalidade dos serviços S3 e EBS do AWS.
- O [armazenamento de tabelas](/azure/cosmos-db/table-storage-how-to-use-nodejs) armazena conjuntos de dados estruturados. O Armazenamento de tabelas é um armazenamento de dados de atributos de chave NoSQL que acelera o desenvolvimento e o acesso a grandes quantidades de dados. Semelhante aos serviços SimpleDB e DynamoDB do AWS.

- O [armazenamento de filas](/azure/storage/queues/storage-nodejs-how-to-use-queues) fornece um sistema de mensagens para processamento de fluxos de trabalho e comunicação entre componentes de serviços de nuvem.

- O [armazenamento de arquivos](/azure/storage/files/storage-java-how-to-use-file-storage) oferece armazenamento compartilhado para aplicativos herdados que usam o protocolo padrão de bloco de mensagens de servidor (SMB). O armazenamento de arquivo é usado de maneira semelhante ao EFS da plataforma AWS.

#### <a name="glacier-and-azure-storage"></a>Glacier e Armazenamento do Azure

O [Armazenamento de Blobs de Arquivo Morto do Azure](/azure/storage/blobs/storage-blob-storage-tiers#archive-access-tier) é comparável ao serviço de armazenamento AWS Glacier. Ele se destina a dados acessados raramente que ficam armazenados por pelo menos 180 dias e podem tolerar várias horas de latência de recuperação.

Para dados que são acessados com pouca frequência, mas devem estar disponíveis imediatamente quando acessados, a [Camada de Armazenamento de Blobs Fria do Azure](/azure/storage/blobs/storage-blob-storage-tiers#cool-access-tier) fornece armazenamento mais barato do que o armazenamento de blobs padrão. Esta camada de armazenamento é comparável ao AWS S3 – serviço de armazenamento de Acesso Não Frequente.

#### <a name="see-also"></a>Consulte também

- [Lista de verificação de desempenho e escalabilidade do Armazenamento do Microsoft Azure](/azure/storage/common/storage-performance-checklist)

- [Guia de segurança do Armazenamento do Azure](/azure/storage/common/storage-security-guide)

- [Práticas recomendadas para uso das CDNs (redes de distribuição de conteúdo)](/azure/architecture/best-practices/cdn)

### <a name="networking"></a>Rede

#### <a name="elastic-load-balancing-azure-load-balancer-and-azure-application-gateway"></a>Elastic Load Balancing, Azure Load Balancer e Gateway de Aplicativo do Azure

Os equivalentes do Azure aos dois serviços Elastic Load Balancing são:

- [Load Balancer](https://azure.microsoft.com/documentation/articles/load-balancer-overview/) - Fornece os mesmos recursos que o Classic Load Balancer do AWS, permitindo distribuir o tráfego por várias VMs no nível da rede. Ele também fornece recursos de failover.

- [Gateway de Aplicativo](https://azure.microsoft.com/documentation/articles/application-gateway-introduction/) - Oferece roteamento com base em regra no nível de aplicativo comparável ao Application Load Balancer do AWS.

#### <a name="route-53-azure-dns-and-azure-traffic-manager"></a>Route 53, DNS do Azure e Gerenciador de Tráfego do Azure

No AWS, o Route 53 fornece serviços de gerenciamento de nomes DNS e roteamento de tráfego em nível de DNS e failover. No Azure, isso é feito por dois serviços:

- O [DNS do Azure](https://azure.microsoft.com/documentation/services/dns/) fornece gerenciamento de domínio e de DNS.

- O [Gerenciador de Tráfego][traffic-manager] fornece recursos de failover, balanceamento de carga e roteamento de tráfego de nível de DNS.

#### <a name="direct-connect-and-azure-expressroute"></a>Conexão direta e Azure ExpressRoute

O Azure fornece conexões dedicadas site a site semelhantes por meio do seu serviço [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/). O ExpressRoute permite conectar a rede local diretamente a recursos do Azure usando uma conexão de rede privada dedicada. O Azure também oferece [conexões VPN site a site](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/) mais convencionais, com menor custo.

#### <a name="see-also"></a>Consulte também

- [Criar uma Rede Virtual usando o portal do Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/)

- [Planejar e projetar Redes Virtuais do Azure](https://azure.microsoft.com/documentation/articles/virtual-network-vnet-plan-design-arm/)

- [Práticas recomendadas de segurança da Rede do Azure](https://azure.microsoft.com/documentation/articles/azure-security-network-security-best-practices/)

### <a name="database-services"></a>Serviços de banco de dados

#### <a name="rds-and-azure-relational-database-services"></a>Serviços de banco de dados relacional do Azure e RDS

O Azure fornece vários serviços de outro banco de dados relacional que serão o equivalente do Serviço de banco de dados relacional (RDS) do AWS.

- [Banco de Dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
- [Banco de Dados do Azure para MySQL](https://docs.microsoft.com/azure/mysql/overview)
- [Banco de Dados do Azure para PostgreSQL](https://docs.microsoft.com/azure/postgresql/overview)

Outros mecanismos de bancos de dados, como [SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/), [Oracle](https://azure.microsoft.com/campaigns/oracle/) e [MySQL](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-classic-mysql-2008r2/) podem ser implantados com a utilização de Instâncias de VM do Azure.

Os custos de RDS do AWS são determinados pela quantidade de recursos de hardware usados pela instância, como CPU, RAM, armazenamento e largura de banda de rede. Nos serviços de banco de dados do Azure, o custo depende do tamanho do seu banco de dados, das conexões simultâneas e dos níveis de produtividade.

#### <a name="see-also"></a>Consulte também

- [Tutoriais do Banco de Dados SQL do Azure](https://azure.microsoft.com/documentation/articles/sql-database-explore-tutorials/)

- [Configurar a replicação geográfica para o Banco de Dados SQL do Azure com o portal do Azure](https://azure.microsoft.com/documentation/articles/sql-database-geo-replication-portal/)

- [Introdução ao Cosmos DB: um banco de dados NoSQL JSON](/azure/cosmos-db/sql-api-introduction)

- [Como usar o armazenamento de Tabela do Azure com Node.js](https://azure.microsoft.com/documentation/articles/storage-nodejs-how-to-use-table-storage/)

### <a name="security-and-identity"></a>Segurança e identidade

#### <a name="directory-service-and-azure-active-directory"></a>Serviço de diretório e Azure Active Directory

O Azure divide os serviços de diretório em:

- [Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-whatis/): serviço de gerenciamento de identidades e diretórios baseado em nuvem.

- [B2B do Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-b2b-collaboration-overview/): habilita o acesso aos seus aplicativos corporativos com identidades gerenciadas pelo parceiro.

- [B2C do Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-b2c-overview/): serviço que oferece suporte ao gerenciamento de usuário e de logon único para aplicativos destinados ao consumidor.

- [Azure Active Directory Domain Services](https://azure.microsoft.com/documentation/articles/active-directory-ds-overview/): serviço de controlador de domínio hospedado que permite a funcionalidade de gerenciamento de usuário e associação de domínio compatível com o Active Directory.

#### <a name="web-application-firewall"></a>Firewall do aplicativo Web

Além do [Firewall de Aplicativo Web do Gateway de Aplicativo](/azure/application-gateway/waf-overview), você também pode usar firewalls de aplicativos Web fornecidos por terceiros, como o [Barracuda Networks](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/).

#### <a name="see-also"></a>Consulte também

- [Introdução à segurança do Microsoft Azure](/azure/security)

- [Práticas recomendadas de Gerenciamento de Identidade do Azure e segurança de controle de acesso](/azure/security/azure-security-identity-management-best-practices)

### <a name="application-and-messaging-services"></a>Serviços de aplicativos e de mensagens

#### <a name="simple-email-service"></a>Simple Email Service

O Simple Email Service (SES) do AWS envia emails de notificação, transacionais ou de marketing. No Azure, as soluções de terceiros, como o [SendGrid](https://sendgrid.com/partners/azure/), fornecem serviços de email.

#### <a name="simple-queueing-service"></a>Simple Queueing Service

O Simple Queueing Service (SQS) do AWS fornece um sistema de mensagens para conectar aplicativos, serviços e dispositivos dentro da plataforma AWS. O Azure tem dois serviços com funcionalidade semelhante:

- [Armazenamento de Filas](/azure/storage/queues/storage-nodejs-how-to-use-queues): um serviço de mensagens em nuvem que permite a comunicação entre componentes de aplicativos na plataforma Azure.

- [Barramento de Serviço](https://azure.microsoft.com/services/service-bus/): um sistema de mensagens mais robusto para conectar aplicativos, serviços e dispositivos. Usando a [retransmissão do Barramento de Serviço](/azure/service-bus-relay/relay-what-is-it) relacionada, o Barramento de Serviço também pode se conectar a aplicativos e serviços hospedados remotamente.

#### <a name="device-farm"></a>Device Farm

O Device Farm do AWS fornece serviços de teste de vários dispositivos. No Azure, o [Xamarin Test Cloud](https://www.xamarin.com/test-cloud) fornece testes de front-end semelhantes entre dispositivos para dispositivos móveis.

Além dos testes de front-end, o [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/) fornece recursos de teste de back-end para ambientes Linux e Windows.

#### <a name="see-also"></a>Consulte também

- [Como usar o Armazenamento de Fila do Node.js](/azure/storage/queues/storage-nodejs-how-to-use-queues)

- [Como usar filas do Barramento de Serviço](/azure/service-bus-messaging/service-bus-nodejs-how-to-use-queues)

### <a name="analytics-and-big-data"></a>Análises e big data

[O Cortana Intelligence Suite](https://azure.microsoft.com/suites/cortana-intelligence-suite/) é o pacote de produtos e serviços do Azure projetados para capturar, organizar, analisar e visualizar grandes quantidades de dados. O pacote Cortana consiste nos seguintes serviços:

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) - Distribuição gerenciada do Apache, que inclui Hadoop, Spark, Storm ou HBase.

- [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) : fornece funcionalidade de orquestração e pipeline de dados.

- [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/): armazenamento de dados relacionais em grande escala.

- [Data Lake Store](https://azure.microsoft.com/documentation/services/data-lake-store/): armazenamento em grande escala otimizado para cargas de trabalho de análise de Big Data.

- [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/): usado para criar e aplicar a análise preditiva em dados.

- [Stream Analytics](https://azure.microsoft.com/documentation/services/stream-analytics/): análise de dados em tempo real.

- [Data Lake Analytics](https://azure.microsoft.com/documentation/articles/data-lake-analytics-overview/): serviço de análise de larga escala, otimizado para trabalhar com o Data Lake Store

- [PowerBI](https://powerbi.microsoft.com/): usado para visualização de dados de energia.

#### <a name="see-also"></a>Consulte também

- [Galeria do Cortana Intelligence](https://gallery.cortanaintelligence.com/)

- [Noções básicas sobre soluções de Big Data da Microsoft](https://msdn.microsoft.com/library/dn749804.aspx)

- [Blog do Azure Data Lake e do Azure HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/)

### <a name="internet-of-things"></a>Internet das coisas

#### <a name="see-also"></a>Consulte também

- [Introdução ao Hub IoT do Azure](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/)

- [Comparação entre Hub IoT e Hubs de Eventos](https://azure.microsoft.com/documentation/articles/iot-hub-compare-event-hubs/)

### <a name="mobile-services"></a>Serviços móveis

#### <a name="notifications"></a>Notificações

Os Hubs de Notificação não dão suporte ao envio de SMS ou mensagens de email; por isso, são necessários serviços de terceiros para esses tipos de entrega.

#### <a name="see-also"></a>Consulte também

- [Criar um aplicativo para Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-android-get-started/)

- [Autenticação e Autorização nos Aplicativos Móveis do Azure](https://azure.microsoft.com/documentation/articles/app-service-mobile-auth/)

- [Como enviar notificações por push com Hubs de Notificação do Azure](https://azure.microsoft.com/documentation/articles/notification-hubs-android-push-notification-google-fcm-get-started/)

### <a name="management-and-monitoring"></a>Gerenciamento e monitoramento

#### <a name="see-also"></a>Consulte também

- [Diretrizes de monitoramento e diagnóstico](https://azure.microsoft.com/documentation/articles/best-practices-monitoring/)

- [Práticas recomendadas para a criação de modelos do Azure Resource Manager](https://azure.microsoft.com/documentation/articles/resource-manager-template-best-practices/)

- [Modelos de Início Rápido do Azure Resource Manager](https://azure.microsoft.com/documentation/templates/)

## <a name="next-steps"></a>Próximas etapas

- [Introdução ao Azure](https://azure.microsoft.com/get-started/)

- [Arquiteturas de solução do Azure](https://azure.microsoft.com/solutions/architecture/)

- [Arquiteturas de Referência do Azure](https://azure.microsoft.com/documentation/articles/guidance-architecture/)

<!-- links -->

[paired-regions]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[traffic-manager]: /azure/traffic-manager/

<!-- markdownlint-enable MD024 -->
