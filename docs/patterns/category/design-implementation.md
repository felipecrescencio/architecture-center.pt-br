---
title: "Padrões de design e implementação"
description: "Um bom design abrange fatores como a consistência e a coerência no design do componente e implantação, facilidade de manutenção para simplificar a administração e desenvolvimento e capacidade de reutilização para permitir que componentes e subsistemas para ser usado em outros aplicativos e em outros cenários. As decisões tomadas durante a fase de design e implementação tem um grande impacto sobre a qualidade e o custo total de propriedade de aplicativos e serviços hospedados pela nuvem."
keywords: "padrão de design"
author: dragon119
ms.date: 06/23/2017
pnp.series.title: Cloud Design Patterns
ms.openlocfilehash: 3d6e528b8c88c6fcc265d6425dcdae6fae5166fb
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2017
---
# <a name="design-and-implementation-patterns"></a><span data-ttu-id="e9188-105">Padrões de design e implementação</span><span class="sxs-lookup"><span data-stu-id="e9188-105">Design and Implementation patterns</span></span>

<span data-ttu-id="e9188-106">Um bom design abrange fatores como a consistência e a coerência no design do componente e implantação, facilidade de manutenção para simplificar a administração e desenvolvimento e capacidade de reutilização para permitir que componentes e subsistemas para ser usado em outros aplicativos e em outros cenários.</span><span class="sxs-lookup"><span data-stu-id="e9188-106">Good design encompasses factors such as consistency and coherence in component design and deployment, maintainability to simplify administration and development, and reusability to allow components and subsystems to be used in other applications and in other scenarios.</span></span> <span data-ttu-id="e9188-107">As decisões tomadas durante a fase de design e implementação tem um grande impacto sobre a qualidade e o custo total de propriedade de aplicativos e serviços hospedados pela nuvem.</span><span class="sxs-lookup"><span data-stu-id="e9188-107">Decisions made during the design and implementation phase have a huge impact on the quality and the total cost of ownership of cloud hosted applications and services.</span></span>

| <span data-ttu-id="e9188-108">Padrão</span><span class="sxs-lookup"><span data-stu-id="e9188-108">Pattern</span></span> | <span data-ttu-id="e9188-109">Resumo</span><span class="sxs-lookup"><span data-stu-id="e9188-109">Summary</span></span> |
| ------- | ------- |
| [<span data-ttu-id="e9188-110">Embaixador</span><span class="sxs-lookup"><span data-stu-id="e9188-110">Ambassador</span></span>](../ambassador.md) | <span data-ttu-id="e9188-111">Crie serviços auxiliares que enviam solicitações de rede em nome de um consumidor de serviço ou aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e9188-111">Create helper services that send network requests on behalf of a consumer service or application.</span></span> |
| [<span data-ttu-id="e9188-112">Camada anticorrupção</span><span class="sxs-lookup"><span data-stu-id="e9188-112">Anti-Corruption Layer</span></span>](../anti-corruption-layer.md) | <span data-ttu-id="e9188-113">Implemente uma camada de fachada ou adaptador entre um aplicativo moderno e um sistema herdado.</span><span class="sxs-lookup"><span data-stu-id="e9188-113">Implement a façade or adapter layer between a modern application and a legacy system.</span></span> |
| [<span data-ttu-id="e9188-114">Back-ends para Front-ends</span><span class="sxs-lookup"><span data-stu-id="e9188-114">Backends for Frontends</span></span>](../backends-for-frontends.md) | <span data-ttu-id="e9188-115">Crie serviços de back-end separados a serem consumidos por aplicativos de front-end específico ou interfaces.</span><span class="sxs-lookup"><span data-stu-id="e9188-115">Create separate backend services to be consumed by specific frontend applications or interfaces.</span></span> |
| [<span data-ttu-id="e9188-116">CQRS</span><span class="sxs-lookup"><span data-stu-id="e9188-116">CQRS</span></span>](../cqrs.md) | <span data-ttu-id="e9188-117">Separar as operações que leem dados de operações que atualizam dados usando interfaces separadas.</span><span class="sxs-lookup"><span data-stu-id="e9188-117">Segregate operations that read data from operations that update data by using separate interfaces.</span></span> |
| [<span data-ttu-id="e9188-118">Consolidação de Recursos de Computação</span><span class="sxs-lookup"><span data-stu-id="e9188-118">Compute Resource Consolidation</span></span>](../compute-resource-consolidation.md) | <span data-ttu-id="e9188-119">Consolidar várias tarefas ou operações em uma única unidade de computação</span><span class="sxs-lookup"><span data-stu-id="e9188-119">Consolidate multiple tasks or operations into a single computational unit</span></span> |
| [<span data-ttu-id="e9188-120">Repositório de configuração externo</span><span class="sxs-lookup"><span data-stu-id="e9188-120">External Configuration Store</span></span>](../external-configuration-store.md) | <span data-ttu-id="e9188-121">Mova as informações de configuração para fora do pacote de implantação de aplicativo para um local centralizado.</span><span class="sxs-lookup"><span data-stu-id="e9188-121">Move configuration information out of the application deployment package to a centralized location.</span></span> |
| [<span data-ttu-id="e9188-122">Agregação de Gateway</span><span class="sxs-lookup"><span data-stu-id="e9188-122">Gateway Aggregation</span></span>](../gateway-aggregation.md) | <span data-ttu-id="e9188-123">Use um gateway para agregar várias solicitações individuais em uma única solicitação.</span><span class="sxs-lookup"><span data-stu-id="e9188-123">Use a gateway to aggregate multiple individual requests into a single request.</span></span> |
| [<span data-ttu-id="e9188-124">Descarregamento de Gateway</span><span class="sxs-lookup"><span data-stu-id="e9188-124">Gateway Offloading</span></span>](../gateway-offloading.md) | <span data-ttu-id="e9188-125">Descarregue a funcionalidade de serviço especializado ou compartilhado para um proxy do gateway.</span><span class="sxs-lookup"><span data-stu-id="e9188-125">Offload shared or specialized service functionality to a gateway proxy.</span></span> |
| [<span data-ttu-id="e9188-126">Roteamento de Gateway</span><span class="sxs-lookup"><span data-stu-id="e9188-126">Gateway Routing</span></span>](../gateway-routing.md) | <span data-ttu-id="e9188-127">Faça o roteamento de solicitações para vários serviços usando um único ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="e9188-127">Route requests to multiple services using a single endpoint.</span></span> |
| [<span data-ttu-id="e9188-128">Eleição de Líder</span><span class="sxs-lookup"><span data-stu-id="e9188-128">Leader Election</span></span>](../leader-election.md) | <span data-ttu-id="e9188-129">Coordene as ações executadas por uma coleção de instâncias de tarefa de colaboração em um aplicativo distribuído elegendo uma instância como a líder que assume a responsabilidade por gerenciar as demais instâncias.</span><span class="sxs-lookup"><span data-stu-id="e9188-129">Coordinate the actions performed by a collection of collaborating task instances in a distributed application by electing one instance as the leader that assumes responsibility for managing the other instances.</span></span> |
| [<span data-ttu-id="e9188-130">Pipes e Filtros</span><span class="sxs-lookup"><span data-stu-id="e9188-130">Pipes and Filters</span></span>](../pipes-and-filters.md) | <span data-ttu-id="e9188-131">Divida uma tarefa que executa processamento complexo em uma série de elementos separados que podem ser reutilizados.</span><span class="sxs-lookup"><span data-stu-id="e9188-131">Break down a task that performs complex processing into a series of separate elements that can be reused.</span></span> |
| [<span data-ttu-id="e9188-132">Sidecar</span><span class="sxs-lookup"><span data-stu-id="e9188-132">Sidecar</span></span>](../sidecar.md) | <span data-ttu-id="e9188-133">Implante os componentes de um aplicativo em um processo ou contêiner separado para fornecer isolamento e encapsulamento.</span><span class="sxs-lookup"><span data-stu-id="e9188-133">Deploy components of an application into a separate process or container to provide isolation and encapsulation.</span></span> |
| [<span data-ttu-id="e9188-134">Hospedagem de Conteúdo Estático</span><span class="sxs-lookup"><span data-stu-id="e9188-134">Static Content Hosting</span></span>](../static-content-hosting.md) | <span data-ttu-id="e9188-135">Implante o conteúdo estático para um serviço de armazenamento baseado em nuvem que pode enviá-las diretamente para o cliente.</span><span class="sxs-lookup"><span data-stu-id="e9188-135">Deploy static content to a cloud-based storage service that can deliver them directly to the client.</span></span> |
| [<span data-ttu-id="e9188-136">Strangler</span><span class="sxs-lookup"><span data-stu-id="e9188-136">Strangler</span></span>](../strangler.md) | <span data-ttu-id="e9188-137">Migre incrementalmente um sistema herdado substituindo gradualmente partes específicas de funcionalidade por serviços e aplicativos novos.</span><span class="sxs-lookup"><span data-stu-id="e9188-137">Incrementally migrate a legacy system by gradually replacing specific pieces of functionality with new applications and services.</span></span> |