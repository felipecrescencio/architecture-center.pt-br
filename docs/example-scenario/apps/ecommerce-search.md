---
title: Mecanismo de pesquisa de produto inteligente para comércio eletrônico
description: Fornece uma experiência de pesquisa de nível internacional em um aplicativo de comércio eletrônico.
author: jelledruyts
ms.date: 09/14/2018
ms.openlocfilehash: f18e9fd3705c24da71da747c46ab42f263fd06af
ms.sourcegitcommit: b2a4eb132857afa70201e28d662f18458865a48e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48818734"
---
# <a name="intelligent-product-search-engine-for-e-commerce"></a><span data-ttu-id="54fda-103">Mecanismo de pesquisa de produto inteligente para comércio eletrônico</span><span class="sxs-lookup"><span data-stu-id="54fda-103">Intelligent product search engine for e-commerce</span></span>

<span data-ttu-id="54fda-104">Esse cenário de exemplo mostra como o uso de um serviço de pesquisa dedicado pode aumentar drasticamente a relevância dos resultados da pesquisa para seus clientes de comércio eletrônico.</span><span class="sxs-lookup"><span data-stu-id="54fda-104">This example scenario shows how using a dedicated search service can dramatically increase the relevance of search results for your e-commerce customers.</span></span>

<span data-ttu-id="54fda-105">A pesquisa é o mecanismo principal por meio do qual os clientes encontram e, em última análise, compram produtos. Portanto, é essencial que os resultados da pesquisa sejam relevantes para a _intenção_ de consulta de pesquisa e que a experiência de pesquisa de ponta a ponta corresponda à dos gigantes de pesquisa, fornecendo resultados quase instantâneos, análise linguística, correspondência de localização geográfica, filtragem, facetas, preenchimento automático, realce de ocorrências etc.</span><span class="sxs-lookup"><span data-stu-id="54fda-105">Search is the primary mechanism through which customers find and ultimately purchase products, making it essential that search results are relevant to the _intent_ of the search query, and that the end-to-end search experience matches that of search giants by providing near-instant results, linguistic analysis, geo-location matching, filtering, faceting, auto-complete, hit highlighting etc.</span></span>

<span data-ttu-id="54fda-106">Imagine um aplicativo Web de comércio eletrônico típico, com dados de produtos armazenados em um banco de dados relacional como o SQL Server ou o Banco de Dados SQL.</span><span class="sxs-lookup"><span data-stu-id="54fda-106">Imagine a typical e-commerce web application with product data stored in a relational database like SQL Server or Azure SQL Database.</span></span> <span data-ttu-id="54fda-107">As consultas de pesquisa normalmente são tratadas no banco de dados usando consultas do `LIKE` ou recursos de [Pesquisa de Texto Completo][docs-sql-fts].</span><span class="sxs-lookup"><span data-stu-id="54fda-107">Search queries are often handled inside the database using `LIKE` queries or [Full-Text Search][docs-sql-fts] features.</span></span> <span data-ttu-id="54fda-108">Usando o [Azure Search][docs-search] em vez disso, você libera seu banco de dados operacional do processamento de consultas e pode facilmente começar a tirar proveito de recursos de implementação difícil que fornecem aos clientes a melhor experiência de pesquisa possível.</span><span class="sxs-lookup"><span data-stu-id="54fda-108">By using [Azure Search][docs-search] instead, you free up your operational database from the query processing and you can easily start taking advantage of those hard-to-implement features that provide your customers with the best possible search experience.</span></span> <span data-ttu-id="54fda-109">Além disso, como o Azure Search é um componente de PaaS (plataforma como serviço), você não precisa se preocupar em gerenciar a infraestrutura ou se tornar um especialista em pesquisa.</span><span class="sxs-lookup"><span data-stu-id="54fda-109">Also, because Azure Search is a platform as a service (PaaS) component, you don't have to worry about managing infrastructure or becoming a search expert.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="54fda-110">Casos de uso relevantes</span><span class="sxs-lookup"><span data-stu-id="54fda-110">Relevant use cases</span></span>

<span data-ttu-id="54fda-111">Estes outros casos de uso têm padrões de design semelhante:</span><span class="sxs-lookup"><span data-stu-id="54fda-111">These other uses cases have similar design patterns:</span></span>

* <span data-ttu-id="54fda-112">Localizar listagens de imóveis ou lojas perto da localização física do usuário.</span><span class="sxs-lookup"><span data-stu-id="54fda-112">Finding real estate listings or stores near the user's physical location.</span></span>
* <span data-ttu-id="54fda-113">Pesquisar artigos em um site de notícias ou procurar resultados esportivos, com preferência mais elevada para obter informações mais _recentes_.</span><span class="sxs-lookup"><span data-stu-id="54fda-113">Searching for articles in a news site or looking for sports results, with a higher preference for more _recent_ information.</span></span>
* <span data-ttu-id="54fda-114">Pesquisar em grandes repositórios de organizações _voltadas em documentos_ como criadores de políticas e notários.</span><span class="sxs-lookup"><span data-stu-id="54fda-114">Searching through large repositories for _document-centric_ organizations like policy makers and notaries.</span></span>

<span data-ttu-id="54fda-115">Em última análise, _qualquer_ aplicativo que tenha alguma funcionalidade de pesquisa poderá se beneficiar de um serviço de pesquisa dedicado.</span><span class="sxs-lookup"><span data-stu-id="54fda-115">Ultimately _any_ application that has some form of search functionality can benefit from having a dedicated search service.</span></span>

## <a name="architecture"></a><span data-ttu-id="54fda-116">Arquitetura</span><span class="sxs-lookup"><span data-stu-id="54fda-116">Architecture</span></span>

![Visão geral da arquitetura dos componentes envolvidos em um mecanismo de pesquisa de produto inteligente para comércio eletrônico do Azure][architecture]

<span data-ttu-id="54fda-118">Este cenário aborda uma solução de comércio eletrônico em que os clientes podem pesquisar em um catálogo de produtos.</span><span class="sxs-lookup"><span data-stu-id="54fda-118">This scenario covers an e-commerce solution where customers can search through a product catalog.</span></span>
1. <span data-ttu-id="54fda-119">Os clientes navegam até o **aplicativo Web de comércio eletrônico** por meio de qualquer dispositivo.</span><span class="sxs-lookup"><span data-stu-id="54fda-119">Customers navigate to the **e-commerce web application** from any device.</span></span>
2. <span data-ttu-id="54fda-120">O catálogo de produtos é mantido em um **Banco de Dados SQL** para processamento transacional.</span><span class="sxs-lookup"><span data-stu-id="54fda-120">The product catalog is maintained in an **Azure SQL Database** for transactional processing.</span></span>
3. <span data-ttu-id="54fda-121">O Azure Search usa um **indexador de pesquisa** para manter seu índice de pesquisa atualizado automaticamente por meio do controle de alterações integrado.</span><span class="sxs-lookup"><span data-stu-id="54fda-121">Azure Search uses a **search indexer** to automatically keep its search index up-to-date through integrated change tracking.</span></span>
4. <span data-ttu-id="54fda-122">As consultas de pesquisa do cliente são descarregadas para o serviço **Azure Search**, que processa a consulta e retorna os resultados mais relevantes.</span><span class="sxs-lookup"><span data-stu-id="54fda-122">Customer's search queries are offloaded to the **Azure Search** service, which processes the query and returns the most relevant results.</span></span>
5. <span data-ttu-id="54fda-123">Como alternativa a uma experiência de pesquisa baseada na Web, os clientes também podem usar um **bot de conversa** em mídias sociais ou diretamente de assistentes digitais para pesquisar produtos e refinar a consulta de pesquisa e os resultados de forma incremental.</span><span class="sxs-lookup"><span data-stu-id="54fda-123">As an alternative to a web-based search experience, customers can also use a **conversational bot** in social media or straight from digital assistants to search for products and incrementally refine their search query and results.</span></span>
6. <span data-ttu-id="54fda-124">Opcionalmente, o recurso de **Pesquisa Cognitiva** pode ser usado para aplicar inteligência artificial e tornar o processamento ainda mais inteligente.</span><span class="sxs-lookup"><span data-stu-id="54fda-124">Optionally, the **Cognitive Search** feature can be used to apply artificial intelligence for even smarter processing.</span></span>

### <a name="components"></a><span data-ttu-id="54fda-125">Componentes</span><span class="sxs-lookup"><span data-stu-id="54fda-125">Components</span></span>

* <span data-ttu-id="54fda-126">Os [Serviços de Aplicativo - Aplicativos Web][docs-webapps] hospedam os aplicativos Web, permitindo o dimensionamento automático e alta disponibilidade sem a necessidade de gerenciar a infraestrutura.</span><span class="sxs-lookup"><span data-stu-id="54fda-126">[App Services - Web Apps][docs-webapps] hosts web applications allowing auto-scale and high availability without having to manage infrastructure.</span></span>
* <span data-ttu-id="54fda-127">O [Banco de Dados SQL][docs-sql-database] é um serviço gerenciado de banco de dados relacional de uso geral no Microsoft Azure que dá suporte a estruturas como XML, JSON, espacial e dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="54fda-127">[SQL Database][docs-sql-database] is a general-purpose relational database-managed service in Microsoft Azure that supports structures such as relational data, JSON, spatial, and XML.</span></span>
* <span data-ttu-id="54fda-128">O [Azure Search][docs-search] é uma solução de pesquisa como serviço na nuvem que oferece uma experiência de pesquisa avançada para conteúdo privado e heterogêneo em aplicativos Web, móveis e empresariais.</span><span class="sxs-lookup"><span data-stu-id="54fda-128">[Azure Search][docs-search] is a search-as-a-service cloud solution that provides a rich search experience over private, heterogenous content in web, mobile, and enterprise applications.</span></span>
* <span data-ttu-id="54fda-129">O [Serviço de Bot][docs-botservice] fornece ferramentas para compilar, testar, implantar e gerenciar bots inteligentes.</span><span class="sxs-lookup"><span data-stu-id="54fda-129">[Bot Service][docs-botservice] provides tools to build, test, deploy, and manage intelligent bots.</span></span>
* <span data-ttu-id="54fda-130">Os [Serviços Cognitivos][docs-cognitive] permitem usar algoritmos inteligentes para ver, ouvir, falar, entender e interpretar as necessidades do usuário por meio de métodos naturais de comunicação.</span><span class="sxs-lookup"><span data-stu-id="54fda-130">[Cognitive Services][docs-cognitive] lets you use intelligent algorithms to see, hear, speak, understand, and interpret your user needs through natural methods of communication.</span></span>

### <a name="alternatives"></a><span data-ttu-id="54fda-131">Alternativas</span><span class="sxs-lookup"><span data-stu-id="54fda-131">Alternatives</span></span>

* <span data-ttu-id="54fda-132">Você pode usar recursos de **pesquisa no banco de dados**, por exemplo, por meio da pesquisa de texto completo do SQL Server. Porém, nesse caso, o repositório transacional também processa consultas (aumentando a necessidade de capacidade de processamento) e os recursos de pesquisa no banco de dados são mais limitados.</span><span class="sxs-lookup"><span data-stu-id="54fda-132">You could use **in-database search** capabilities, for example, through SQL Server full-text search, but then your transactional store also processes queries (increasing the need for processing power) and the search capabilities inside the database are more limited.</span></span>
* <span data-ttu-id="54fda-133">Você pode hospedar o software livre [Apache Lucene][apache-lucene] (no qual o Azure Search se baseia) em Máquinas Virtuais do Azure, mas, dessa forma, volta a gerenciar IaaS (infraestrutura como Serviço) e não se beneficia dos muitos recursos adicionais que o Azure Search oferece em comparação com o Lucene.</span><span class="sxs-lookup"><span data-stu-id="54fda-133">You could host the open-source [Apache Lucene][apache-lucene] (on which Azure Search is built upon) on Azure Virtual Machines, but then you are back to managing Infrastructure-as-a-Service (IaaS) and don't benefit from the many features that Azure Search provides on top of Lucene.</span></span>
* <span data-ttu-id="54fda-134">Você também pode considerar a implantação de [Elastic Search][elastic-marketplace] do Azure Marketplace, que é um produto de pesquisa alternativo e eficaz de um fornecedor de terceiros, mas, também nesse caso, executa uma carga de trabalho de IaaS.</span><span class="sxs-lookup"><span data-stu-id="54fda-134">You could also consider deploying [Elastic Search][elastic-marketplace] from the Azure Marketplace, which is an alternative and capable search product from a third-party vendor, but also in this case you are running an IaaS workload.</span></span>

<span data-ttu-id="54fda-135">Outras opções para a camada de dados incluem:</span><span class="sxs-lookup"><span data-stu-id="54fda-135">Other options for the data tier include:</span></span>

* <span data-ttu-id="54fda-136">[Cosmos DB](/azure/cosmos-db/introduction) ‒ banco de dados multimodelo globalmente distribuído da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="54fda-136">[Cosmos DB](/azure/cosmos-db/introduction) - Microsoft's globally distributed, multi-model database.</span></span> <span data-ttu-id="54fda-137">O Cosmos DB fornece uma plataforma para executar outros modelos de dados, como Mongo DB, Cassandra, dados do gráfico ou armazenamento de tabela simples.</span><span class="sxs-lookup"><span data-stu-id="54fda-137">Costmos DB provides a platform to run other data models such as Mongo DB, Cassandra, Graph data, or simple table storage.</span></span> <span data-ttu-id="54fda-138">O Azure Search também dá suporte à indexação de dados diretamente do Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="54fda-138">Azure Search also supports indexing the data from Cosmos DB directly.</span></span>

## <a name="considerations"></a><span data-ttu-id="54fda-139">Considerações</span><span class="sxs-lookup"><span data-stu-id="54fda-139">Considerations</span></span>

### <a name="scalability"></a><span data-ttu-id="54fda-140">Escalabilidade</span><span class="sxs-lookup"><span data-stu-id="54fda-140">Scalability</span></span>

<span data-ttu-id="54fda-141">A [camada de preço][search-tier] do serviço Azure Search não determina os recursos disponíveis, mas é usada principalmente para [planejamento de capacidade][search-capacity], pois define o armazenamento máximo que você obtém e quantas partições e réplicas pode provisionar.</span><span class="sxs-lookup"><span data-stu-id="54fda-141">The [pricing tier][search-tier] of the Azure Search service doesn't determine the available features but is used mainly for [capacity planning][search-capacity] as it defines the maximum storage you get and how many partitions and replicas you can provision.</span></span> <span data-ttu-id="54fda-142">**Partições** permitem que você indexe mais documentos e obtenha taxas de transferência de gravação mais altas, enquanto **réplicas** fornecem mais QPS (Consultas por Segundo) e Alta Disponibilidade.</span><span class="sxs-lookup"><span data-stu-id="54fda-142">**Partitions** allow you to index more documents and get higher write throughputs, whereas **replicas** provide more Queries-Per-Second (QPS) and High Availability.</span></span>

<span data-ttu-id="54fda-143">Você pode alterar dinamicamente o número de partições e réplicas, mas não é possível alterar a camada de preço. Portanto, você deve considerar cuidadosamente a camada certa para sua carga de trabalho de destino.</span><span class="sxs-lookup"><span data-stu-id="54fda-143">You can dynamically change the number of partitions and replicas but it's not possible to change the pricing tier, so you should carefully consider the right tier for your target workload.</span></span> <span data-ttu-id="54fda-144">Se precisar alterar a camada mesmo assim, você precisará provisionar um novo serviço lado a lado e recarregar os índices, podendo então apontar seus aplicativos no novo serviço.</span><span class="sxs-lookup"><span data-stu-id="54fda-144">If you need to change the tier anyway, you will need to provision a new service side by side and reload your indexes there, at which point you can point your applications at the new service.</span></span>

### <a name="availability"></a><span data-ttu-id="54fda-145">Disponibilidade</span><span class="sxs-lookup"><span data-stu-id="54fda-145">Availability</span></span>

<span data-ttu-id="54fda-146">O Azure Search fornece um [SLA de 99,9% de disponibilidade][search-sla] para _leituras_ (ou seja, consultas) se você tem pelo menos duas réplicas e para _atualizações_(ou seja, atualização dos índices de pesquisa) se você tem pelo menos três réplicas.</span><span class="sxs-lookup"><span data-stu-id="54fda-146">Azure Search provides a [99.9% availability SLA][search-sla] for _reads_ (that is, querying) if you have at least two replicas, and for _updates_ (that is, updating the search indexes) if you have at least three replicas.</span></span> <span data-ttu-id="54fda-147">Portanto, você deverá provisionar pelo menos duas réplicas se quiser que seus clientes possam _pesquisar_ de maneira confiável e três se _alterações reais no índice_ também precisarem ser consideradas operações de alta disponibilidade.</span><span class="sxs-lookup"><span data-stu-id="54fda-147">Therefore you should provision at least two replicas if you want your customers to be able to _search_ reliably, and 3 if actual _changes to the index_ should also be considered high availability operations.</span></span>

<span data-ttu-id="54fda-148">Se houver necessidade de fazer alterações significativas no índice sem tempo de inatividade (por exemplo, alterar tipos de dados, excluir ou renomear campos), o índice precisará ser recriado.</span><span class="sxs-lookup"><span data-stu-id="54fda-148">If there is a need to make breaking changes to the index without downtime (for example, changing data types, deleting or renaming fields), the index will need to be rebuilt.</span></span> <span data-ttu-id="54fda-149">Assim como ocorre com a alteração da camada de serviço, isso significa criar um novo índice, populá-lo novamente com os dados e atualizando seus aplicativos para apontar para o novo índice.</span><span class="sxs-lookup"><span data-stu-id="54fda-149">Similar to changing service tier, this means creating a new index, repopulating it with the data, and then updating your applications to point at the new index.</span></span>

### <a name="security"></a><span data-ttu-id="54fda-150">Segurança</span><span class="sxs-lookup"><span data-stu-id="54fda-150">Security</span></span>

<span data-ttu-id="54fda-151">O Azure Search está em conformidade com muitas [normas de privacidade de dados e segurança][search-security], o que permite que seja usado na maioria dos setores.</span><span class="sxs-lookup"><span data-stu-id="54fda-151">Azure Search is compliant with many [security and data privacy standards][search-security], which makes it possible to be used in most industries.</span></span>

<span data-ttu-id="54fda-152">Para proteger o acesso ao serviço, o Azure Search usa dois tipos de chaves: **chaves de administração**, que permitem que você execute _qualquer_ tarefa em relação ao serviço, e **chaves de consulta**, que só podem ser usadas para operações somente leitura, como consultas.</span><span class="sxs-lookup"><span data-stu-id="54fda-152">For securing access to the service, Azure Search uses two types of keys: **admin keys**, which allow you to perform _any_ task against the service, and **query keys**, which can only be used for read-only operations like querying.</span></span> <span data-ttu-id="54fda-153">Normalmente, o aplicativo que executa a pesquisa não atualiza o índice. Portanto, ele deve ser configurado apenas com uma chave de consulta, não com uma chave de administração (especialmente se a pesquisa é realizada de um dispositivo de usuário final, como um script em execução em um navegador da Web).</span><span class="sxs-lookup"><span data-stu-id="54fda-153">Typically, the application that performs the search does not update the index, so it should only be configured with a query key and not an admin key (especially if the search is performed from an end-user device like script running in a web browser).</span></span>

### <a name="search-relevance"></a><span data-ttu-id="54fda-154">Relevância de pesquisa</span><span class="sxs-lookup"><span data-stu-id="54fda-154">Search Relevance</span></span>

<span data-ttu-id="54fda-155">O êxito de seu aplicativo de comércio eletrônico depende em grande parte da relevância dos resultados da pesquisa para seus clientes.</span><span class="sxs-lookup"><span data-stu-id="54fda-155">How successful your e-commerce application is depends largely on the relevance of the search results to your customers.</span></span> <span data-ttu-id="54fda-156">Ajuste cuidadosamente o serviço de pesquisa para fornecer resultados ideais com base na pesquisa de usuário ou recorra a recursos internos, como [análise de tráfego de pesquisa][search-analysis], para entender os padrões de pesquisa do cliente e tomar decisões com base nos dados.</span><span class="sxs-lookup"><span data-stu-id="54fda-156">Carefully tuning your search service to provide optimal results based on user research, or relying on built-in features such as [search traffic analysis][search-analysis] to understand your customer's search patterns allows you to make decisions based on data.</span></span>

<span data-ttu-id="54fda-157">Maneiras comuns de ajustar o serviço de pesquisa:</span><span class="sxs-lookup"><span data-stu-id="54fda-157">Typical ways to tune your search service include:</span></span>

* <span data-ttu-id="54fda-158">Usar [perfis de pontuação][search-scoring] para influenciar a relevância dos resultados da pesquisa, por exemplo, com base em qual campo corresponde à consulta, até que ponto os dados são recentes, a distância geográfica até o usuário etc.</span><span class="sxs-lookup"><span data-stu-id="54fda-158">Using [scoring profiles][search-scoring] to influence the relevance of search results, for example, based on which field matched the query, how recent the data is, the geographical distance to the user, ...</span></span>
* <span data-ttu-id="54fda-159">Usar [analisadores de idioma fornecidos pela Microsoft][search-languages], que usam uma pilha de NLP (Processamento de Linguagem Natural) avançada para interpretar melhor as consultas</span><span class="sxs-lookup"><span data-stu-id="54fda-159">Using [Microsoft provided language analyzers][search-languages] that use an advanced Natural Language Processing (NLP) stack to better interpret queries</span></span>
* <span data-ttu-id="54fda-160">Usar [analisadores personalizados][search-analyzers] garantir que seus produtos sejam encontrados corretamente, sobretudo se você deseja pesquisar informações não baseadas em linguagem, como marca e modelo de um produto.</span><span class="sxs-lookup"><span data-stu-id="54fda-160">Using [custom analyzers][search-analyzers] to ensure your products are found correctly, especially if you want to search on non-language based information like a product's make and model.</span></span>

## <a name="deploy-this-scenario"></a><span data-ttu-id="54fda-161">Implantar este cenário</span><span class="sxs-lookup"><span data-stu-id="54fda-161">Deploy this scenario</span></span>

<span data-ttu-id="54fda-162">Para implantar uma versão de comércio eletrônico mais completa deste cenário, você pode seguir este [tutorial passo a passo][end-to-end-walkthrough], que fornece um aplicativo .NET de exemplo que executa um aplicativo simples de compra de tíquetes.</span><span class="sxs-lookup"><span data-stu-id="54fda-162">To deploy a more complete e-commerce version of this scenario, you can follow this [step-by-step tutorial][end-to-end-walkthrough] that provides a .NET sample application that runs a simple ticket purchasing application.</span></span> <span data-ttu-id="54fda-163">Ele também inclui o Azure Search e usa muitos dos recursos discutidos.</span><span class="sxs-lookup"><span data-stu-id="54fda-163">It also includes Azure Search and uses many of the features discussed.</span></span> <span data-ttu-id="54fda-164">Além disso, há um modelo do Resource Manager para automatizar a implantação da maioria dos recursos do Azure.</span><span class="sxs-lookup"><span data-stu-id="54fda-164">Additionally, there is a Resource Manager template to automate the deployment of most of the Azure resources.</span></span>

## <a name="pricing"></a><span data-ttu-id="54fda-165">Preços</span><span class="sxs-lookup"><span data-stu-id="54fda-165">Pricing</span></span>

<span data-ttu-id="54fda-166">Para explorar o custo de executar esse cenário, todos os serviços mencionados acima são pré-configurados na calculadora de custos.</span><span class="sxs-lookup"><span data-stu-id="54fda-166">To explore the cost of running this scenario, all the services mentioned above are pre-configured in the cost calculator.</span></span> <span data-ttu-id="54fda-167">Para ver como o preço seria alterado para o seu uso específico, altere as variáveis apropriadas para que sejam correspondentes ao uso esperado.</span><span class="sxs-lookup"><span data-stu-id="54fda-167">To see how the pricing would change for your particular use case change the appropriate variables to match your expected usage.</span></span>

<span data-ttu-id="54fda-168">Fornecemos três perfis de custo de exemplo com base na quantidade de tráfego que você espera receber:</span><span class="sxs-lookup"><span data-stu-id="54fda-168">We have provided three sample cost profiles based on amount of traffic you expect to get:</span></span>

* <span data-ttu-id="54fda-169">[Pequeno][small-pricing]: neste perfil, usamos um único aplicativo Web `Standard S1` para hospedar o site, a camada gratuita do serviço de Bot do Azure, um único serviço do Azure Search `Basic` e um Banco de Dados SQL `Standard S2`.</span><span class="sxs-lookup"><span data-stu-id="54fda-169">[Small][small-pricing]: In this profile, we're using a single `Standard S1` Web App to host the website, the free tier of the Azure Bot service, a single `Basic` Azure Search service, and a `Standard S2` SQL Database.</span></span>
* <span data-ttu-id="54fda-170">[Médio][medium-pricing]: dimensionamos o aplicativo Web para duas instâncias da camada `Standard S3`, atualizando o Serviço de Pesquisa para uma camada `Standard S1` e usando um Banco de Dados SQL `Standard S6`.</span><span class="sxs-lookup"><span data-stu-id="54fda-170">[Medium][medium-pricing]: Here we are scaling up the Web App to two instances of the `Standard S3` tier, upgrading the Search Service to a `Standard S1` tier, and using a `Standard S6` SQL Database.</span></span>
* <span data-ttu-id="54fda-171">[Grande][large-pricing]: no maior perfil, usamos quatro instâncias de um aplicativo Web `Premium P2V2`, atualizamos o serviço de Bot do Azure para a camada `Standard S1` (com 1.000.000 mensagens nos canais Premium), usamos duas unidades do serviço Azure Search `Standard S3` e um Banco de Dados SQL `Premium P6`.</span><span class="sxs-lookup"><span data-stu-id="54fda-171">[Large][large-pricing]: In the largest profile, we use four instances of a `Premium P2V2` Web App, upgrade the Azure Bot service to the `Standard S1` tier (with 1.000.000 messages in Premium channels), use 2 units of the `Standard S3` Azure Search service, and a `Premium P6` SQL Database.</span></span>

## <a name="related-resources"></a><span data-ttu-id="54fda-172">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="54fda-172">Related resources</span></span>

<span data-ttu-id="54fda-173">Para saber mais sobre o Azure Search, acesse o [centro de documentação][docs-search], confira os [exemplos][search-samples] ou veja um [site de demonstração][search-demo] completo em ação.</span><span class="sxs-lookup"><span data-stu-id="54fda-173">To learn more about Azure Search, visit the [documentation center][docs-search], check out the [samples][search-samples], or see a full fledged [demo site][search-demo] in action.</span></span>

<!-- links -->
[architecture]: ./media/architecture-ecommerce-search.png
[docs-sql-fts]: /sql/relational-databases/search/query-with-full-text-search
[docs-search]: /azure/search/search-what-is-azure-search
[docs-sql-database]: /azure/sql-database/sql-database-technical-overview
[docs-webapps]: /azure/app-service/app-service-web-overview
[docs-botservice]: /azure/bot-service/
[docs-cognitive]: /azure/cognitive-services/
[apache-lucene]: https://lucene.apache.org/
[elastic-marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/elastic.elasticsearch
[end-to-end-walkthrough]: https://github.com/Azure/fta-customerfacingapps/tree/master/ecommerce/articles
[search-sla]: https://go.microsoft.com/fwlink/?LinkId=716855
[search-tier]: /azure/search/search-sku-tier
[search-capacity]: /azure/search/search-capacity-planning
[search-security]: /azure/search/search-security-overview
[search-analysis]: /azure/search/search-traffic-analytics
[search-languages]: /rest/api/searchservice/language-support
[search-analyzers]: /rest/api/searchservice/custom-analyzers-in-azure-search
[search-scoring]: /rest/api/searchservice/add-scoring-profiles-to-a-search-index
[search-samples]: https://azure.microsoft.com/resources/samples/?service=search&sort=0
[search-demo]: https://azjobsdemo.azurewebsites.net/
[small-pricing]: https://azure.com/e/db2672a55b6b4d768ef0060a8d9759bd
[medium-pricing]: https://azure.com/e/a5ad0706c9e74add811e83ef83766a1c
[large-pricing]: https://azure.com/e/57f95a898daa487795bd305599973ee6