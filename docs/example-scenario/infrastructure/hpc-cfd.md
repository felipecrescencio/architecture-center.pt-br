---
title: Executar simulações da dinâmica dos fluidos computacional (CFD) no Azure
description: Execute a dinâmica dos fluidos computacional (CFD) no Azure.
author: mikewarr
ms.date: 09/20/2018
ms.openlocfilehash: 5734e6fe707e3beb5e23f2ad2b4344ba289803bb
ms.sourcegitcommit: b2a4eb132857afa70201e28d662f18458865a48e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48818558"
---
# <a name="running-computational-fluid-dynamics-cfd-simulations-on-azure"></a><span data-ttu-id="06355-103">Executar simulações da dinâmica dos fluidos computacional (CFD) no Azure</span><span class="sxs-lookup"><span data-stu-id="06355-103">Running computational fluid dynamics (CFD) simulations on Azure</span></span>

<span data-ttu-id="06355-104">As simulações da dinâmica dos fluidos computacionais (CFD) exigem um tempo significativo de computação e um hardware especializado.</span><span class="sxs-lookup"><span data-stu-id="06355-104">Computational Fluid Dynamics (CFD) simulations require significant compute time along with specialized hardware.</span></span> <span data-ttu-id="06355-105">À medida que aumenta o uso de cluster, os tempos de simulação e de uso geral da grade crescem, gerando problemas em relação à capacidade reserva e longas filas de espera.</span><span class="sxs-lookup"><span data-stu-id="06355-105">As cluster usage increases, simulation times and overall grid use grow, leading to issues with spare capacity and long queue times.</span></span> <span data-ttu-id="06355-106">A adição de um hardware físico pode custar caro e pode não estar em conformidade com os períodos de altas e baixas de uma empresa.</span><span class="sxs-lookup"><span data-stu-id="06355-106">Adding physical hardware can be expensive, and may not align to the usage peaks and valleys that a business goes through.</span></span> <span data-ttu-id="06355-107">Usando o Azure, muitos desses desafios podem ser superados sem gastos extras.</span><span class="sxs-lookup"><span data-stu-id="06355-107">By taking advantage of Azure, many of these challenges can be overcome with no capital expenditure.</span></span>

<span data-ttu-id="06355-108">O Azure fornece o hardware necessário para executar seus trabalhos da CFD nas máquinas virtuais do CPU e GPU.</span><span class="sxs-lookup"><span data-stu-id="06355-108">Azure provides the hardware you need to run your CFD jobs on both GPU and CPU virtual machines.</span></span> <span data-ttu-id="06355-109">Os tamanhos de VM habilitados para o RDMA (Acesso Remoto Direto à Memória) têm redes com base no InfiniBand do FDR, o que permite a comunicação do MPI (Message Passing Interface) de baixa latência.</span><span class="sxs-lookup"><span data-stu-id="06355-109">RDMA (Remote Direct Memory Access) enabled VM sizes have FDR InfiniBand-based networking which allows for low latency MPI (Message Passing Interface) communication.</span></span> <span data-ttu-id="06355-110">Combinado com o Avere vFXT, que fornece um sistema de arquivos clusterizados de escala empresarial, os clientes podem garantir a melhor taxa de transferência para operações de leitura no Azure.</span><span class="sxs-lookup"><span data-stu-id="06355-110">Combined with the Avere vFXT, which provides an enterprise-scale clustered file system, customers can ensure maximum throughput for read operations in Azure.</span></span>

<span data-ttu-id="06355-111">Para simplificar a criação, o gerenciamento e a otimização de clusters do HPC, o Azure CycleCloud pode ser usado para provisionar os clusters e organizar os dados em cenários na nuvem e híbridos.</span><span class="sxs-lookup"><span data-stu-id="06355-111">To simplify the creation, management, and optimization of HPC clusters, Azure CycleCloud can be used to provision clusters and orchestrate data in both hybrid and cloud scenarios.</span></span> <span data-ttu-id="06355-112">Com o monitoramento dos trabalhos pendentes, o CycleCloud inicia automaticamente a computação sob demanda, na qual você só paga pelo que usa, conectado ao agendador de carga de trabalho de sua preferência.</span><span class="sxs-lookup"><span data-stu-id="06355-112">By monitoring the pending jobs, CycleCloud will automatically launch on-demand compute, where you only pay for what you use, connected to the workload scheduler of your choice.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="06355-113">Casos de uso relevantes</span><span class="sxs-lookup"><span data-stu-id="06355-113">Relevant use cases</span></span>

<span data-ttu-id="06355-114">Considere este cenário para estes setores em que os aplicativos de CFD podem ser usados:</span><span class="sxs-lookup"><span data-stu-id="06355-114">Consider this scenario for these industries where CFD applications could be used:</span></span>

* <span data-ttu-id="06355-115">Aeronáutico</span><span class="sxs-lookup"><span data-stu-id="06355-115">Aeronautics</span></span>
* <span data-ttu-id="06355-116">Automotivo</span><span class="sxs-lookup"><span data-stu-id="06355-116">Automotive</span></span>
* <span data-ttu-id="06355-117">HVAC de construção</span><span class="sxs-lookup"><span data-stu-id="06355-117">Building HVAC</span></span>
* <span data-ttu-id="06355-118">Óleo e gás</span><span class="sxs-lookup"><span data-stu-id="06355-118">Oil and gas</span></span>
* <span data-ttu-id="06355-119">Ciências da vida</span><span class="sxs-lookup"><span data-stu-id="06355-119">Life sciences</span></span>

## <a name="architecture"></a><span data-ttu-id="06355-120">Arquitetura</span><span class="sxs-lookup"><span data-stu-id="06355-120">Architecture</span></span>

![Diagrama da arquitetura][architecture]

<span data-ttu-id="06355-122">Este diagrama mostra uma visão geral de alto nível de um típico design híbrido que oferece o monitoramento dos nós sob demanda no Azure:</span><span class="sxs-lookup"><span data-stu-id="06355-122">This diagram shows a high-level overview of a typical hybrid design providing job monitoring of the on-demand nodes in Azure:</span></span>

1. <span data-ttu-id="06355-123">Conecte-se ao servidor do Azure CycleCloud para configurar o cluster.</span><span class="sxs-lookup"><span data-stu-id="06355-123">Connect to the Azure CycleCloud server to configure the cluster.</span></span>
2. <span data-ttu-id="06355-124">Configure e crie o nó principal do cluster, usando máquinas com o RDMA habilitado para MPI.</span><span class="sxs-lookup"><span data-stu-id="06355-124">Configure and create the cluster head node, using RDMA enabled machines for MPI.</span></span>
3. <span data-ttu-id="06355-125">Adicione e configure o nó principal local.</span><span class="sxs-lookup"><span data-stu-id="06355-125">Add and configure the on-premises head node.</span></span>
4. <span data-ttu-id="06355-126">Caso não haja recursos o suficiente, o Azure CycleCloud irá aumentar (ou reduzir) os recursos de computação no Azure.</span><span class="sxs-lookup"><span data-stu-id="06355-126">If there are insufficient resources, Azure CycleCloud will scale up (or down) compute resources in Azure.</span></span> <span data-ttu-id="06355-127">É possível definir um limite predeterminado para evitar a alocação excessiva.</span><span class="sxs-lookup"><span data-stu-id="06355-127">A predetermined limit can be defined to prevent over allocation.</span></span>
5. <span data-ttu-id="06355-128">Tarefas alocadas para os nós de execução.</span><span class="sxs-lookup"><span data-stu-id="06355-128">Tasks allocated to the execute nodes.</span></span>
6. <span data-ttu-id="06355-129">Dados armazenados em cache no Azure do servidor NFS local.</span><span class="sxs-lookup"><span data-stu-id="06355-129">Data cached in Azure from on-premises NFS server.</span></span>
7. <span data-ttu-id="06355-130">Dados lidos do cache do Avere vFXT para Azure.</span><span class="sxs-lookup"><span data-stu-id="06355-130">Data read in from the Avere vFXT for Azure cache.</span></span>
8. <span data-ttu-id="06355-131">Informações de trabalho e de tarefa retransmitidas para o servidor do Azure CycleCloud.</span><span class="sxs-lookup"><span data-stu-id="06355-131">Job and task information relayed to the Azure CycleCloud server.</span></span>

### <a name="components"></a><span data-ttu-id="06355-132">Componentes</span><span class="sxs-lookup"><span data-stu-id="06355-132">Components</span></span>

* <span data-ttu-id="06355-133">O [Azure CycleCloud][cyclecloud] é uma ferramenta para criar, gerenciar, operar e otimizar clusters de HPC e Big Compute no Azure.</span><span class="sxs-lookup"><span data-stu-id="06355-133">[Azure CycleCloud][cyclecloud] a tool for creating, managing, operating, and optimizing HPC and Big Compute clusters in Azure.</span></span>
* <span data-ttu-id="06355-134">O [Avere vFXT no Azure][avere] é usado para fornecer um sistema de arquivos clusterizados de escala empresarial desenvolvido para a nuvem.</span><span class="sxs-lookup"><span data-stu-id="06355-134">[Avere vFXT on Azure][avere] is used to provide an enterprise-scale clustered file system built for the cloud.</span></span>
* <span data-ttu-id="06355-135">As [Máquinas Virtuais (VMs) do Microsoft Azure][vms] são usadas para criar um conjunto estático de instâncias de computação.</span><span class="sxs-lookup"><span data-stu-id="06355-135">[Azure Virtual Machines (VMs)][vms] are used to create a static set of compute instances.</span></span>
* <span data-ttu-id="06355-136">Os [Conjuntos de Dimensionamento de Máquinas Virtuais (conjunto de dimensionamento de máquinas virtuais)][vmss] fornece um grupo de VMs idênticas que podem ser ampliadas ou reduzidas no Azure CycleCloud.</span><span class="sxs-lookup"><span data-stu-id="06355-136">[Virtual Machine Scale Sets (virtual machine scale set)][vmss] provide a group of identical VMs capable of being scaled up or down by Azure CycleCloud.</span></span>
* <span data-ttu-id="06355-137">As [contas do Armazenamento do Azure](/azure/storage/common/storage-introduction) são usadas para a sincronização e a retenção de dados.</span><span class="sxs-lookup"><span data-stu-id="06355-137">[Azure Storage accounts](/azure/storage/common/storage-introduction) are used for synchronization and data retention.</span></span>
* <span data-ttu-id="06355-138">As [Redes Virtuais](/azure/virtual-network/virtual-networks-overview) permitem vários tipos de recursos do Azure, como Máquinas Virtuais (VMs) do Microsoft Azure, a fim de se comunicar de forma segura com a Internet, com as redes locais e com outras VMs.</span><span class="sxs-lookup"><span data-stu-id="06355-138">[Virtual Networks](/azure/virtual-network/virtual-networks-overview) enable many types of Azure resources, such as Azure Virtual Machines (VMs), to securely communicate with each other, the internet, and on-premises networks.</span></span>

### <a name="alternatives"></a><span data-ttu-id="06355-139">Alternativas</span><span class="sxs-lookup"><span data-stu-id="06355-139">Alternatives</span></span>

<span data-ttu-id="06355-140">Os clientes também podem usar o Azure CycleCloud para criar uma grade inteiramente no Azure.</span><span class="sxs-lookup"><span data-stu-id="06355-140">Customers can also use Azure CycleCloud to create a grid entirely in Azure.</span></span> <span data-ttu-id="06355-141">Nessa configuração, o servidor do Azure CycleCloud é executado dentro da assinatura do Azure.</span><span class="sxs-lookup"><span data-stu-id="06355-141">In this setup, the Azure CycleCloud server is run within your Azure subscription.</span></span>

<span data-ttu-id="06355-142">Para uma abordagem de aplicativo moderno, em que o gerenciamento de um agendador de carga de trabalho não é necessário, o [Lote do Azure][batch] pode ajudar.</span><span class="sxs-lookup"><span data-stu-id="06355-142">For a modern application approach where management of a workload scheduler is not needed, [Azure Batch][batch] can help.</span></span> <span data-ttu-id="06355-143">O Lote do Azure pode executar com eficiência aplicativos HPC (computação de alto desempenho) e paralelos em grande escala na nuvem.</span><span class="sxs-lookup"><span data-stu-id="06355-143">Azure Batch can run large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="06355-144">O Lote do Azure permite definir os recursos de computação do Azure para executar seus aplicativos em paralelo ou em escala sem configurar manualmente ou gerenciar a infraestrutura.</span><span class="sxs-lookup"><span data-stu-id="06355-144">Azure Batch allows you to define the Azure compute resources to execute your applications in parallel or at scale without manually configuring or managing infrastructure.</span></span> <span data-ttu-id="06355-145">O Lote do Azure agenda as tarefas que requerem muita computação e adiciona e remove dinamicamente os recursos de computação com base em seus requisitos.</span><span class="sxs-lookup"><span data-stu-id="06355-145">Azure Batch schedules compute-intensive tasks and dynamically adds and removes compute resources based on your requirements.</span></span>

### <a name="scalability-and-security"></a><span data-ttu-id="06355-146">Escalabilidade e Segurança</span><span class="sxs-lookup"><span data-stu-id="06355-146">Scalability, and Security</span></span>

<span data-ttu-id="06355-147">O dimensionamento dos nós de execução no Azure CycleCloud pode ser feito manualmente ou usando o dimensionamento automático.</span><span class="sxs-lookup"><span data-stu-id="06355-147">Scaling the execute nodes on Azure CycleCloud can be accomplished either manually or using autoscaling.</span></span> <span data-ttu-id="06355-148">Para saber mais, veja [Dimensionamento Automático do CycleCloud][cycle-scale].</span><span class="sxs-lookup"><span data-stu-id="06355-148">For more information, see [CycleCloud Autoscaling][cycle-scale].</span></span>

<span data-ttu-id="06355-149">Confira orientações gerais sobre como criar soluções seguras na [Documentação de segurança do Azure][security].</span><span class="sxs-lookup"><span data-stu-id="06355-149">For general guidance on designing secure solutions, see the [Azure security documentation][security].</span></span>

## <a name="deploy-this-scenario"></a><span data-ttu-id="06355-150">Implantar este cenário</span><span class="sxs-lookup"><span data-stu-id="06355-150">Deploy this scenario</span></span>

<span data-ttu-id="06355-151">Alguns pré-requisitos são necessários antes da implantação no Azure.</span><span class="sxs-lookup"><span data-stu-id="06355-151">Before deploying in Azure, some pre-requisites are required.</span></span> <span data-ttu-id="06355-152">Siga estas etapas antes de implantar o modelo do Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="06355-152">Follow these steps before deploying the Resource Manager template:</span></span>
1. <span data-ttu-id="06355-153">Crie uma [entidade de serviço][cycle-svcprin] para recuperar o appId, displayName, nome, senha e locatário.</span><span class="sxs-lookup"><span data-stu-id="06355-153">Create a [service principal][cycle-svcprin] for retrieving the appId, displayName, name, password, and tenant.</span></span>
2. <span data-ttu-id="06355-154">Gere um [par de chaves SSH][cycle-ssh] para entrar com segurança no servidor CycleCloud.</span><span class="sxs-lookup"><span data-stu-id="06355-154">Generate an [SSH key pair][cycle-ssh] to sign in securely to the CycleCloud server.</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FCycleCloudCommunity%2Fcyclecloud_arm%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

3. <span data-ttu-id="06355-155">[Faça logon no servidor do CycleCloud][cycle-login] para configurar e criar um novo cluster.</span><span class="sxs-lookup"><span data-stu-id="06355-155">[Log into the CycleCloud server][cycle-login] to configure and create a new cluster.</span></span>
4. <span data-ttu-id="06355-156">[Crie um cluster][cycle-create].</span><span class="sxs-lookup"><span data-stu-id="06355-156">[Create a cluster][cycle-create].</span></span>

<span data-ttu-id="06355-157">O Cache do Avere é uma solução opcional que pode aumentar drasticamente a taxa de transferência de leitura dos dados do trabalho do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="06355-157">The Avere Cache is an optional solution that can drastically increase read throughput for the application job data.</span></span> <span data-ttu-id="06355-158">O Avere vFXT para Azure resolve o problema da execução desses aplicativos de HPC na nuvem e aproveita os dados armazenados no local ou no Armazenamento de Blobs do Azure.</span><span class="sxs-lookup"><span data-stu-id="06355-158">Avere vFXT for Azure solves the problem of running these enterprise HPC applications in the cloud while leveraging data stored on-premises or in Azure Blob storage.</span></span>

<span data-ttu-id="06355-159">Para organizações que pretendem usar uma infraestrutura híbrida com computação na nuvem e armazenamento no local, os aplicativos de HPC podem “disparar” para o Azure usando os dados armazenados nos dispositivos NAS e processar CPUs virtuais conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="06355-159">For organizations that are planning for a hybrid infrastructure with both on-premises storage and cloud computing, HPC applications can “burst” into Azure using data stored in NAS devices and spin up virtual CPUs as needed.</span></span> <span data-ttu-id="06355-160">O conjunto de dados nunca é completamente movido para a nuvem.</span><span class="sxs-lookup"><span data-stu-id="06355-160">The data set is never moved completely into the cloud.</span></span> <span data-ttu-id="06355-161">Os bytes solicitados são armazenados temporariamente em cache usando um cluster do Avere durante o processamento.</span><span class="sxs-lookup"><span data-stu-id="06355-161">The requested bytes are temporarily cached using an Avere cluster during processing.</span></span>

<span data-ttu-id="06355-162">Para definir e configurar uma instalação do Avere vFXT, siga o [Guia de Instalação e Configuração do Avere][avere].</span><span class="sxs-lookup"><span data-stu-id="06355-162">To set up and configure an Avere vFXT installation, follow the [Avere Setup and Configuration guide][avere].</span></span>

## <a name="pricing"></a><span data-ttu-id="06355-163">Preços</span><span class="sxs-lookup"><span data-stu-id="06355-163">Pricing</span></span>

<span data-ttu-id="06355-164">O custo de executar uma implantação de HPC usando o servidor do CycleCloud varia de acordo com uma série de fatores.</span><span class="sxs-lookup"><span data-stu-id="06355-164">The cost of running an HPC implementation using CycleCloud server will vary depending on a number of factors.</span></span> <span data-ttu-id="06355-165">Por exemplo, o CycleCloud é cobrado pelo tempo de computação que é usado, com o servidor do CycleCloud e Master normalmente sendo constantemente alocado e em execução.</span><span class="sxs-lookup"><span data-stu-id="06355-165">For example, CycleCloud is charged by the amount of compute time that is used, with the Master and CycleCloud server typically being constantly allocated and running.</span></span> <span data-ttu-id="06355-166">O custo para executar os nós de execução depende do tempo de operação deles, bem como do tamanho que é usado.</span><span class="sxs-lookup"><span data-stu-id="06355-166">The cost of running the Execute nodes will depend on how long these are up and running as well as what size is used.</span></span> <span data-ttu-id="06355-167">Também se aplicam os encargos normais de armazenamento e rede do Azure.</span><span class="sxs-lookup"><span data-stu-id="06355-167">The normal Azure charges for storage and networking also apply.</span></span>

<span data-ttu-id="06355-168">Este cenário mostra como os aplicativos de CFD podem ser executados no Azure, as máquinas exigirão a funcionalidade de RDMA, que só está disponível em tamanhos específicos de máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="06355-168">This scenario shows how CFD applications can be run in Azure, so the machines will require RDMA functionality, which is only available on specific VM sizes.</span></span> <span data-ttu-id="06355-169">Veja a seguir exemplos de custos gerados para um conjunto de dimensionamento alocado continuamente oito horas por dia durante um mês, com saída de dados de 1 TB.</span><span class="sxs-lookup"><span data-stu-id="06355-169">The following are examples of costs that could be incurred for a scale set that is allocated continuously for eight hours per day for one month, with data egress of 1 TB.</span></span> <span data-ttu-id="06355-170">Isso inclui também o preço do servidor do Azure CycleCloud e da instalação do Avere vFXT para Azure:</span><span class="sxs-lookup"><span data-stu-id="06355-170">It also includes pricing for the Azure CycleCloud server and the Avere vFXT for Azure install:</span></span>

* <span data-ttu-id="06355-171">Região: Europa Setentrional</span><span class="sxs-lookup"><span data-stu-id="06355-171">Region: North Europe</span></span>
* <span data-ttu-id="06355-172">Servidor do Azure CycleCloud: 1x D3 Standard (4x CPUs, 14 GB de Memória, HDD Standard de 32 GB)</span><span class="sxs-lookup"><span data-stu-id="06355-172">Azure CycleCloud Server: 1 x Standard D3 (4 x CPUs, 14 GB Memory, Standard HDD 32 GB)</span></span>
* <span data-ttu-id="06355-173">Servidor do Azure CycleCloud Master: 1x D12 Standard (4x CPUs, 28 GB de Memória, HDD Standard de 32 GB)</span><span class="sxs-lookup"><span data-stu-id="06355-173">Azure CycleCloud Master Server: 1 x Standard D12 v (4 x CPUs, 28 GB Memory, Standard HDD 32 GB)</span></span>
* <span data-ttu-id="06355-174">Matriz do Nó do Azure CycleCloud: 10x H16r Standard (16x CPUs, 112 GB de Memória)</span><span class="sxs-lookup"><span data-stu-id="06355-174">Azure CycleCloud Node Array: 10 x Standard H16r (16 x CPUs, 112 GB Memory)</span></span>
* <span data-ttu-id="06355-175">Avere vFXT no Cluster do Azure: 3x D16s v3 (200 GB OS, disco de dados SSD Premium de 1-TB)</span><span class="sxs-lookup"><span data-stu-id="06355-175">Avere vFXT on Azure Cluster: 3 x D16s v3 (200 GB OS, Premium SSD 1-TB data disk)</span></span>
* <span data-ttu-id="06355-176">Saída de Dados: 1 TB</span><span class="sxs-lookup"><span data-stu-id="06355-176">Data Egress: 1 TB</span></span>

<span data-ttu-id="06355-177">Analise esta [estimativa de preço][pricing] do hardware listado acima.</span><span class="sxs-lookup"><span data-stu-id="06355-177">Review this [price estimate][pricing] for the hardware listed above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06355-178">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="06355-178">Next Steps</span></span>

<span data-ttu-id="06355-179">Depois de implantar o exemplo, saiba mais sobre o [Azure CycleCloud][cyclecloud].</span><span class="sxs-lookup"><span data-stu-id="06355-179">Once you've deployed the sample, learn more about [Azure CycleCloud][cyclecloud].</span></span>

## <a name="related-resources"></a><span data-ttu-id="06355-180">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="06355-180">Related resources</span></span>

* <span data-ttu-id="06355-181">[Instâncias de Máquina Compatíveis com RDMA][rdma]</span><span class="sxs-lookup"><span data-stu-id="06355-181">[RDMA Capable Machine Instances][rdma]</span></span>
* <span data-ttu-id="06355-182">[Personalizar uma Máquina Virtual da Instância do RDMA][rdma-custom]</span><span class="sxs-lookup"><span data-stu-id="06355-182">[Customizing an RDMA Instance VM][rdma-custom]</span></span>

<!-- links -->
[architecture]: ./media/architecture-hpc-cfd.png
[calculator]: https://azure.com/e/
[availability]: /azure/architecture/checklist/availability
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[vmss]: /azure/virtual-machine-scale-sets/overview
[cyclecloud]: /azure/cyclecloud/
[rdma]: /azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances
[gpu]: /azure/virtual-machines/windows/sizes-gpu
[hpcsizes]: /azure/virtual-machines/windows/sizes-hpc
[vms]: /azure/virtual-machines/
[low-pri]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority
[batch]: /azure/batch/
[avere]: https://github.com/Azure/Avere/blob/master/README.md
[cycle-prereq]: /azure/cyclecloud/quickstart-install-cyclecloud#prerequisites
[cycle-svcprin]: /azure/cyclecloud/quickstart-install-cyclecloud#service-principal
[cycle-ssh]: /azure/cyclecloud/quickstart-install-cyclecloud#ssh-keypair
[cycle-login]: /azure/cyclecloud/quickstart-install-cyclecloud#log-into-the-cyclecloud-application-server
[cycle-create]: /azure/cyclecloud/quickstart-create-and-run-cluster
[rdma]: /azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances
[rdma-custom]: /azure/virtual-machines/linux/classic/rdma-cluster#customize-the-vm
[pricing]: https://azure.com/e/53030a04a2ab47a289156e2377a4247a
[cycle-scale]: /azure/cyclecloud/autoscale