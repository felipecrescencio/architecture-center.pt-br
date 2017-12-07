---
title: "Nenhum antipadrão de cache"
description: Buscar repetidamente os mesmos dados pode reduzir o desempenho e a escalabilidade.
author: dragon119
ms.date: 06/05/2017
ms.openlocfilehash: eb6b69c13b8954ae2efb1da96dec05bc1c13d18b
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2017
---
# <a name="no-caching-antipattern"></a><span data-ttu-id="b2fea-103">Nenhum antipadrão de cache</span><span class="sxs-lookup"><span data-stu-id="b2fea-103">No Caching antipattern</span></span>

<span data-ttu-id="b2fea-104">Em um aplicativo de nuvem que trata muitas solicitações simultâneas, buscar repetidamente os mesmos dados pode reduzir o desempenho e a escalabilidade.</span><span class="sxs-lookup"><span data-stu-id="b2fea-104">In a cloud application that handles many concurrent requests, repeatedly fetching the same data can reduce performance and scalability.</span></span> 

## <a name="problem-description"></a><span data-ttu-id="b2fea-105">Descrição do problema</span><span class="sxs-lookup"><span data-stu-id="b2fea-105">Problem description</span></span>

<span data-ttu-id="b2fea-106">Quando os dados não estão armazenados em cache, isso pode causar vários comportamentos indesejados, incluindo:</span><span class="sxs-lookup"><span data-stu-id="b2fea-106">When data is not cached, it can cause a number of undesirable behaviors, including:</span></span>

- <span data-ttu-id="b2fea-107">Buscando repetidamente as mesmas informações de um recurso que é caro para o acesso, em termos de latência ou sobrecarga de E/S.</span><span class="sxs-lookup"><span data-stu-id="b2fea-107">Repeatedly fetching the same information from a resource that is expensive to access, in terms of I/O overhead or latency.</span></span>
- <span data-ttu-id="b2fea-108">Construindo repetidamente os mesmos objetos ou estruturas de dados para várias solicitações.</span><span class="sxs-lookup"><span data-stu-id="b2fea-108">Repeatedly constructing the same objects or data structures for multiple requests.</span></span>
- <span data-ttu-id="b2fea-109">Fazendo chamadas excessivas para um serviço remoto que tem uma cota de serviço e limita os clientes após um determinado limite.</span><span class="sxs-lookup"><span data-stu-id="b2fea-109">Making excessive calls to a remote service that has a service quota and throttles clients past a certain limit.</span></span>

<span data-ttu-id="b2fea-110">Por sua vez, esses problemas podem resultar em tempos de resposta baixos, mais contenção no armazenamento de dados e baixa escalabilidade.</span><span class="sxs-lookup"><span data-stu-id="b2fea-110">In turn, these problems can lead to poor response times, increased contention in the data store, and poor scalability.</span></span>

<span data-ttu-id="b2fea-111">O exemplo a seguir usa o Entity Framework para se conectar a um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-111">The following example uses Entity Framework to connect to a database.</span></span> <span data-ttu-id="b2fea-112">Cada solicitação de cliente resulta em uma chamada para o banco de dados, mesmo se várias solicitações estejam buscando exatamente os mesmos dados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-112">Every client request results in a call to the database, even if multiple requests are fetching exactly the same data.</span></span> <span data-ttu-id="b2fea-113">O custo de solicitações repetidas, em termos de sobrecarga de E/S e encargos de acesso de dados, pode se acumular rapidamente.</span><span class="sxs-lookup"><span data-stu-id="b2fea-113">The cost of repeated requests, in terms of I/O overhead and data access charges, can accumulate quickly.</span></span>

```csharp
public class PersonRepository : IPersonRepository
{
    public async Task<Person> GetAsync(int id)
    {
        using (var context = new AdventureWorksContext())
        {
            return await context.People
                .Where(p => p.Id == id)
                .FirstOrDefaultAsync()
                .ConfigureAwait(false);
        }
    }
}
```

<span data-ttu-id="b2fea-114">Você pode encontrar o exemplo completo [aqui][sample-app].</span><span class="sxs-lookup"><span data-stu-id="b2fea-114">You can find the complete sample [here][sample-app].</span></span>

<span data-ttu-id="b2fea-115">Esse antipadrão geralmente ocorre porque:</span><span class="sxs-lookup"><span data-stu-id="b2fea-115">This antipattern typically occurs because:</span></span>

- <span data-ttu-id="b2fea-116">Não usar um cache é mais simples de implementar, e funciona bem em cargas baixas.</span><span class="sxs-lookup"><span data-stu-id="b2fea-116">Not using a cache is simpler to implement, and it works fine under low loads.</span></span> <span data-ttu-id="b2fea-117">O cache torna o código mais complicado.</span><span class="sxs-lookup"><span data-stu-id="b2fea-117">Caching makes the code more complicated.</span></span> 
- <span data-ttu-id="b2fea-118">As vantagens e desvantagens do uso de um cache não são claramente compreendidas.</span><span class="sxs-lookup"><span data-stu-id="b2fea-118">The benefits and drawbacks of using a cache are not clearly understood.</span></span>
- <span data-ttu-id="b2fea-119">Há uma preocupação quanto à sobrecarga de manter a precisão e a atualização de dados armazenados em cache.</span><span class="sxs-lookup"><span data-stu-id="b2fea-119">There is concern about the overhead of maintaining the accuracy and freshness of cached data.</span></span>
- <span data-ttu-id="b2fea-120">Um aplicativo foi migrado de um sistema local, em que a latência de rede não era um problema e o sistema foi executado em hardwares mais caros de alto desempenho, para que o cache não fosse considerado no design original.</span><span class="sxs-lookup"><span data-stu-id="b2fea-120">An application was migrated from an on-premises system, where network latency was not an issue, and the system ran on expensive high-performance hardware, so caching wasn't considered in the original design.</span></span>
- <span data-ttu-id="b2fea-121">Os desenvolvedores não estão cientes de que o cache é uma possibilidade em um determinado cenário.</span><span class="sxs-lookup"><span data-stu-id="b2fea-121">Developers aren't aware that caching is a possibility in a given scenario.</span></span> <span data-ttu-id="b2fea-122">Por exemplo, os desenvolvedores podem não pensar em usar ETags ao implementar uma API da Web.</span><span class="sxs-lookup"><span data-stu-id="b2fea-122">For example, developers may not think of using ETags when implementing a web API.</span></span>

## <a name="how-to-fix-the-problem"></a><span data-ttu-id="b2fea-123">Como corrigir o problema</span><span class="sxs-lookup"><span data-stu-id="b2fea-123">How to fix the problem</span></span>

<span data-ttu-id="b2fea-124">A estratégia de cache mais popular é a estratégia *sob demanda* ou *cache-aside*.</span><span class="sxs-lookup"><span data-stu-id="b2fea-124">The most popular caching strategy is the *on-demand* or *cache-aside* strategy.</span></span>

- <span data-ttu-id="b2fea-125">Na leitura, o aplicativo tenta ler os dados do cache.</span><span class="sxs-lookup"><span data-stu-id="b2fea-125">On read, the application tries to read the data from the cache.</span></span> <span data-ttu-id="b2fea-126">Se os dados não estiverem no cache, o aplicativo recupera-o da fonte de dados e o adiciona ao cache.</span><span class="sxs-lookup"><span data-stu-id="b2fea-126">If the data isn't in the cache, the application retrieves it from the data source and adds it to the cache.</span></span>
- <span data-ttu-id="b2fea-127">Na gravação, o aplicativo grava a alteração diretamente na fonte de dados e remove o valor antigo do cache.</span><span class="sxs-lookup"><span data-stu-id="b2fea-127">On write, the application writes the change directly to the data source and removes the old value from the cache.</span></span> <span data-ttu-id="b2fea-128">Ele será recuperado e adicionado ao cache da próxima vez que for necessário.</span><span class="sxs-lookup"><span data-stu-id="b2fea-128">It will be retrieved and added to the cache the next time it is required.</span></span>

<span data-ttu-id="b2fea-129">Essa abordagem é adequada para dados que são alterados com frequência.</span><span class="sxs-lookup"><span data-stu-id="b2fea-129">This approach is suitable for data that changes frequently.</span></span> <span data-ttu-id="b2fea-130">Aqui está o exemplo anterior atualizado para usar o padrão [Cache-Aside] [cache-aside].</span><span class="sxs-lookup"><span data-stu-id="b2fea-130">Here is the previous example updated to use the [Cache-Aside][cache-aside] pattern.</span></span>  

```csharp
public class CachedPersonRepository : IPersonRepository
{
    private readonly PersonRepository _innerRepository;

    public CachedPersonRepository(PersonRepository innerRepository)
    {
        _innerRepository = innerRepository;
    }

    public async Task<Person> GetAsync(int id)
    {
        return await CacheService.GetAsync<Person>("p:" + id, () => _innerRepository.GetAsync(id)).ConfigureAwait(false);
    }
}

public class CacheService
{
    private static ConnectionMultiplexer _connection;

    public static async Task<T> GetAsync<T>(string key, Func<Task<T>> loadCache, double expirationTimeInMinutes)
    {
        IDatabase cache = Connection.GetDatabase();
        T value = await GetAsync<T>(cache, key).ConfigureAwait(false);
        if (value == null)
        {
            // Value was not found in the cache. Call the lambda to get the value from the database.
            value = await loadCache().ConfigureAwait(false);
            if (value != null)
            {
                // Add the value to the cache.
                await SetAsync(cache, key, value, expirationTimeInMinutes).ConfigureAwait(false);
            }
        }
        return value;
    }
}
```

<span data-ttu-id="b2fea-131">Observe que o método `GetAsync` agora chama a classe `CacheService`, em vez de chamar diretamente o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-131">Notice that the `GetAsync` method now calls the `CacheService` class, rather than calling the database directly.</span></span> <span data-ttu-id="b2fea-132">Primeiro, a classe `CacheService` tenta obter o item do Cache Redis do Azure.</span><span class="sxs-lookup"><span data-stu-id="b2fea-132">The `CacheService` class first tries to get the item from Azure Redis Cache.</span></span> <span data-ttu-id="b2fea-133">Se o valor não for encontrado no Cache Redis, o `CacheService` invoca uma função lambda que foi passada pelo chamador.</span><span class="sxs-lookup"><span data-stu-id="b2fea-133">If the value isn't found in Redis Cache, the `CacheService` invokes a lambda function that was passed to it by the caller.</span></span> <span data-ttu-id="b2fea-134">A função lambda é responsável por buscar os dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-134">The lambda function is responsible for fetching the data from the database.</span></span> <span data-ttu-id="b2fea-135">Essa implementação separa o repositório da solução de cache específica e separa o `CacheService` do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-135">This implementation decouples the repository from the particular caching solution, and decouples the `CacheService` from the database.</span></span> 

## <a name="considerations"></a><span data-ttu-id="b2fea-136">Considerações</span><span class="sxs-lookup"><span data-stu-id="b2fea-136">Considerations</span></span>

- <span data-ttu-id="b2fea-137">Se o cache estiver indisponível, talvez devido a uma falha transitória, não retorne um erro para o cliente.</span><span class="sxs-lookup"><span data-stu-id="b2fea-137">If the cache is unavailable, perhaps because of a transient failure, don't return an error to the client.</span></span> <span data-ttu-id="b2fea-138">Em vez disso, busque os dados da fonte de dados original.</span><span class="sxs-lookup"><span data-stu-id="b2fea-138">Instead, fetch the data from the original data source.</span></span> <span data-ttu-id="b2fea-139">Mas, lembre-se: durante a recuperação do cache, o armazenamento de dados original pode ser inundado com solicitações, resultando em tempos limite e conexões com falha.</span><span class="sxs-lookup"><span data-stu-id="b2fea-139">However, be aware that while the cache is being recovered, the original data store could be swamped with requests, resulting in timeouts and failed connections.</span></span> <span data-ttu-id="b2fea-140">(Afinal, isso é uma das motivações para usar um cache em primeiro lugar.) Use uma técnica, como o [Padrão de Disjuntor][circuit-breaker] para evitar sobrecarregar a fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-140">(After all, this is one of the motivations for using a cache in the first place.) Use a technique such as the [Circuit Breaker pattern][circuit-breaker] to avoid overwhelming the data source.</span></span>

- <span data-ttu-id="b2fea-141">Aplicativos que armazenam em cache dados não estáticos devem ser criados para oferecer suporte à consistência eventual.</span><span class="sxs-lookup"><span data-stu-id="b2fea-141">Applications that cache nonstatic data should be designed to support eventual consistency.</span></span>

- <span data-ttu-id="b2fea-142">Para APIs da Web, você pode oferecer suporte ao cache do cliente, incluindo um cabeçalho Cache-Control nas mensagens de solicitação e resposta e usar ETags para identificar as versões de objetos.</span><span class="sxs-lookup"><span data-stu-id="b2fea-142">For web APIs, you can support client-side caching by including a Cache-Control header in request and response messages, and using ETags to identify versions of objects.</span></span> <span data-ttu-id="b2fea-143">Para obter mais informações, confira [Implementação da API][api-implementation].</span><span class="sxs-lookup"><span data-stu-id="b2fea-143">For more information, see [API implementation][api-implementation].</span></span>

- <span data-ttu-id="b2fea-144">Você não precisa colocar em cache as entidades completas.</span><span class="sxs-lookup"><span data-stu-id="b2fea-144">You don't have to cache entire entities.</span></span> <span data-ttu-id="b2fea-145">Se grande parte de uma entidade for estática, mas somente uma pequena parte for alterada com frequência, coloque em cache os elementos estáticos e recupere os elementos dinâmicos da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-145">If most of an entity is static but only a small piece changes frequently, cache the static elements and retrieve the dynamic elements from the data source.</span></span> <span data-ttu-id="b2fea-146">Essa abordagem pode ajudar a reduzir o volume de E/S que está sendo executado em relação à fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-146">This approach can help to reduce the volume of I/O being performed against the data source.</span></span>

- <span data-ttu-id="b2fea-147">Em alguns casos, se os dados voláteis forem de curta duração, pode ser útil para colocar em cache.</span><span class="sxs-lookup"><span data-stu-id="b2fea-147">In some cases, if volatile data is short-lived, it can be useful to cache it.</span></span> <span data-ttu-id="b2fea-148">Por exemplo, considere um dispositivo que envia continuamente as atualizações de status.</span><span class="sxs-lookup"><span data-stu-id="b2fea-148">For example, consider a device that continually sends status updates.</span></span> <span data-ttu-id="b2fea-149">Pode fazer sentido armazenar em cache essas informações conforme elas chegam e não gravá-las em um repositório persistente.</span><span class="sxs-lookup"><span data-stu-id="b2fea-149">It might make sense to cache this information as it arrives, and not write it to a persistent store at all.</span></span>  

- <span data-ttu-id="b2fea-150">Para impedir que os dados se tornem obsoletos, muitas soluções de cache oferecem suporte a períodos de validade configurável, para que os dados sejam removidos do cache automaticamente após um intervalo especificado.</span><span class="sxs-lookup"><span data-stu-id="b2fea-150">To prevent data from becoming stale, many caching solutions support configurable expiration periods, so that data is automatically removed from the cache after a specified interval.</span></span> <span data-ttu-id="b2fea-151">Talvez seja necessário ajustar o tempo de expiração para seu cenário.</span><span class="sxs-lookup"><span data-stu-id="b2fea-151">You may need to tune the expiration time for your scenario.</span></span> <span data-ttu-id="b2fea-152">Os dados que são altamente estáticos podem permanecer no cache por períodos mais longos que os dados voláteis que podem se tornar obsoletos rapidamente.</span><span class="sxs-lookup"><span data-stu-id="b2fea-152">Data that is highly static can stay in the cache for longer periods than volatile data that may become stale quickly.</span></span>

- <span data-ttu-id="b2fea-153">Se a solução de cache não fornecer expiração interna, você precisará implementar um processo em segundo plano que ocasionalmente varre o cache, para impedir que ele cresça sem limites.</span><span class="sxs-lookup"><span data-stu-id="b2fea-153">If the caching solution doesn't provide built-in expiration, you may need to implement a background process that occasionally sweeps the cache, to prevent it from growing without limits.</span></span> 

- <span data-ttu-id="b2fea-154">Além de armazenar em cache os dados de uma fonte de dados externa, você pode usar o cache para salvar os resultados de cálculos complexos.</span><span class="sxs-lookup"><span data-stu-id="b2fea-154">Besides caching data from an external data source, you can use caching to save the results of complex computations.</span></span> <span data-ttu-id="b2fea-155">Antes de fazer isso, no entanto, instrumente o aplicativo para determinar se o aplicativo está realmente associado à CPU.</span><span class="sxs-lookup"><span data-stu-id="b2fea-155">Before you do that, however, instrument the application to determine whether the application is really CPU bound.</span></span>

- <span data-ttu-id="b2fea-156">Pode ser útil preparar o cache quando o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="b2fea-156">It might be useful to prime the cache when the application starts.</span></span> <span data-ttu-id="b2fea-157">Popule o cache com os dados que são mais prováveis de serem utilizados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-157">Populate the cache with the data that is most likely to be used.</span></span>

- <span data-ttu-id="b2fea-158">Inclua sempre instrumentação que detecta ocorrências no cache e perdas no cache.</span><span class="sxs-lookup"><span data-stu-id="b2fea-158">Always include instrumentation that detects cache hits and cache misses.</span></span> <span data-ttu-id="b2fea-159">Use essas informações para ajustar as políticas de cache, tais como: quais dados armazenar em cache e por quanto tempo manter os dados no cache antes de expirar.</span><span class="sxs-lookup"><span data-stu-id="b2fea-159">Use this information to tune caching policies, such what data to cache, and how long to hold data in the cache before it expires.</span></span>

- <span data-ttu-id="b2fea-160">Se a falta de armazenamento em cache for um gargalo, adicionar o cache pode aumentar o volume de solicitações de tal forma que o front-end da Web fique sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="b2fea-160">If the lack of caching is a bottleneck, then adding caching may increase the volume of requests so much that the web front end becomes overloaded.</span></span> <span data-ttu-id="b2fea-161">Os clientes podem começar a receber erros HTTP 503 (Serviço Indisponível).</span><span class="sxs-lookup"><span data-stu-id="b2fea-161">Clients may start to receive HTTP 503 (Service Unavailable) errors.</span></span> <span data-ttu-id="b2fea-162">Essas são indicações de que você deve escalar horizontalmente o front-end.</span><span class="sxs-lookup"><span data-stu-id="b2fea-162">These are an indication that you should scale out the front end.</span></span>

## <a name="how-to-detect-the-problem"></a><span data-ttu-id="b2fea-163">Como detectar o problema</span><span class="sxs-lookup"><span data-stu-id="b2fea-163">How to detect the problem</span></span>

<span data-ttu-id="b2fea-164">Você pode executar as etapas a seguir para ajudar a identificar se a falta de cache está causando problemas de desempenho:</span><span class="sxs-lookup"><span data-stu-id="b2fea-164">You can perform the following steps to help identify whether lack of caching is causing performance problems:</span></span>

1. <span data-ttu-id="b2fea-165">Examine o design do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b2fea-165">Review the application design.</span></span> <span data-ttu-id="b2fea-166">Faça um inventário de todos os repositórios de dados que o aplicativo utiliza.</span><span class="sxs-lookup"><span data-stu-id="b2fea-166">Take an inventory of all the data stores that the application uses.</span></span> <span data-ttu-id="b2fea-167">Para cada um, determine se o aplicativo está usando um cache.</span><span class="sxs-lookup"><span data-stu-id="b2fea-167">For each, determine whether the application is using a cache.</span></span> <span data-ttu-id="b2fea-168">Se possível, determine a frequência com a qual os dados são alterados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-168">If possible, determine how frequently the data changes.</span></span> <span data-ttu-id="b2fea-169">Bons candidatos inicias para o cache incluem dados que são alterados lentamente e dados de referência estática lidos com frequência.</span><span class="sxs-lookup"><span data-stu-id="b2fea-169">Good initial candidates for caching include data that changes slowly, and static reference data that is read frequently.</span></span> 

2. <span data-ttu-id="b2fea-170">Instrumente o aplicativo e monitore o sistema ao vivo para descobrir a frequência com a qual o aplicativo recupera dados ou calcula informações.</span><span class="sxs-lookup"><span data-stu-id="b2fea-170">Instrument the application and monitor the live system to find out how frequently the application retrieves data or calculates information.</span></span>

3. <span data-ttu-id="b2fea-171">Crie um perfil para o aplicativo em um ambiente de teste para capturar métricas de baixo nível sobre a sobrecarga associada com operações de acesso de dados ou outros cálculos frequentemente executados.</span><span class="sxs-lookup"><span data-stu-id="b2fea-171">Profile the application in a test environment to capture low-level metrics about the overhead associated with data access operations or other frequently performed calculations.</span></span>

4. <span data-ttu-id="b2fea-172">Execute o teste de carga em um ambiente de teste para identificar como o sistema responde sob uma carga de trabalho normal e sob uma carga pesada.</span><span class="sxs-lookup"><span data-stu-id="b2fea-172">Perform load testing in a test environment to identify how the system responds under a normal workload and under heavy load.</span></span> <span data-ttu-id="b2fea-173">O teste de carga deve simular o padrão de acesso a dados observado no ambiente de produção usando cargas de trabalho reais.</span><span class="sxs-lookup"><span data-stu-id="b2fea-173">Load testing should simulate the pattern of data access observed in the production environment using realistic workloads.</span></span> 

5. <span data-ttu-id="b2fea-174">Examine as estatísticas de acesso a dados para os armazenamentos de dados subjacentes e examine quantas vezes as mesmas solicitações de dados são repetidas.</span><span class="sxs-lookup"><span data-stu-id="b2fea-174">Examine the data access statistics for the underlying data stores and review how often the same data requests are repeated.</span></span> 


## <a name="example-diagnosis"></a><span data-ttu-id="b2fea-175">Diagnóstico de exemplo</span><span class="sxs-lookup"><span data-stu-id="b2fea-175">Example diagnosis</span></span>

<span data-ttu-id="b2fea-176">As seções a seguir aplicam essas etapas ao aplicativo de exemplo descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b2fea-176">The following sections apply these steps to the sample application described earlier.</span></span>

### <a name="instrument-the-application-and-monitor-the-live-system"></a><span data-ttu-id="b2fea-177">Instrumentar o aplicativo e monitorar o sistema ao vivo</span><span class="sxs-lookup"><span data-stu-id="b2fea-177">Instrument the application and monitor the live system</span></span>

<span data-ttu-id="b2fea-178">Instrumente o aplicativo e monitore-o para obter informações sobre as solicitações específicas feitas pelos usuários enquanto o aplicativo está em produção.</span><span class="sxs-lookup"><span data-stu-id="b2fea-178">Instrument the application and monitor it to get information about the specific requests that users make while the application is in production.</span></span> 

<span data-ttu-id="b2fea-179">A imagem a seguir mostra o monitoramento dados capturados por [New Relic][NewRelic] durante um teste de carga.</span><span class="sxs-lookup"><span data-stu-id="b2fea-179">The following image shows monitoring data captured by [New Relic][NewRelic] during a load test.</span></span> <span data-ttu-id="b2fea-180">Nesse caso, a única operação HTTP GET executada é `Person/GetAsync`.</span><span class="sxs-lookup"><span data-stu-id="b2fea-180">In this case, the only HTTP GET operation performed is `Person/GetAsync`.</span></span> <span data-ttu-id="b2fea-181">Mas, em um ambiente de produção ao vivo, saber a frequência relativa com a qual cada solicitação é executada pode fornecer informações sobre os recursos que devem ser armazenados em cache.</span><span class="sxs-lookup"><span data-stu-id="b2fea-181">But in a live production environment, knowing the relative frequency that each request is performed can give you insight into which resources should be cached.</span></span>

![New Relic mostrando as solicitações do servidor para o aplicativo CachingDemo][NewRelic-server-requests]

<span data-ttu-id="b2fea-183">Se precisar de uma análise mais profunda, você pode usar um criador de perfil para capturar dados de desempenho de baixo nível em um ambiente de teste (não no sistema de produção).</span><span class="sxs-lookup"><span data-stu-id="b2fea-183">If you need a deeper analysis, you can use a profiler to capture low-level performance data in a test environment (not the production system).</span></span> <span data-ttu-id="b2fea-184">Examine as métricas, como taxas de solicitação de E/S, o uso da memória e a utilização da CPU.</span><span class="sxs-lookup"><span data-stu-id="b2fea-184">Look at metrics such as I/O request rates, memory usage, and CPU utilization.</span></span> <span data-ttu-id="b2fea-185">Essas métricas podem mostrar um grande número de solicitações para um armazenamento de dados ou serviço, ou processamento repetido que realiza o mesmo cálculo.</span><span class="sxs-lookup"><span data-stu-id="b2fea-185">These metrics may show a large number of requests to a data store or service, or repeated processing that performs the same calculation.</span></span> 

### <a name="load-test-the-application"></a><span data-ttu-id="b2fea-186">Fazer teste de carga no aplicativo</span><span class="sxs-lookup"><span data-stu-id="b2fea-186">Load test the application</span></span>

<span data-ttu-id="b2fea-187">O gráfico a seguir mostra os resultados do teste de carga para o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="b2fea-187">The following graph shows the results of load testing the sample application.</span></span> <span data-ttu-id="b2fea-188">O teste de carga simula uma carga de etapa de até 800 usuários executando uma série típica de operações.</span><span class="sxs-lookup"><span data-stu-id="b2fea-188">The load test simulates a step load of up to 800 users performing a typical series of operations.</span></span> 

![Resultados do teste de carga de desempenho para o cenário sem cache][Performance-Load-Test-Results-Uncached]

<span data-ttu-id="b2fea-190">O número de testes bem-sucedidos executados a cada segundo atinge um limite e, como resultado, as solicitações adicionais são desaceleradas.</span><span class="sxs-lookup"><span data-stu-id="b2fea-190">The number of successful tests performed each second reaches a plateau, and additional requests are slowed as a result.</span></span> <span data-ttu-id="b2fea-191">O tempo médio de teste é incrementado continuamente com a carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="b2fea-191">The average test time steadily increases with the workload.</span></span> <span data-ttu-id="b2fea-192">Os níveis do tempo de resposta são desativados quando o usuário realiza picos de carga.</span><span class="sxs-lookup"><span data-stu-id="b2fea-192">The response time levels off once the user load peaks.</span></span>

### <a name="examine-data-access-statistics"></a><span data-ttu-id="b2fea-193">Examinar as estatísticas de acesso de dados</span><span class="sxs-lookup"><span data-stu-id="b2fea-193">Examine data access statistics</span></span>

<span data-ttu-id="b2fea-194">As estatísticas de acesso a dados e outras informações fornecidas por um armazenamento de dados pode fornecer informações úteis, tais como: quais consultas são repetidas com mais frequência.</span><span class="sxs-lookup"><span data-stu-id="b2fea-194">Data access statistics and other information provided by a data store can give useful information, such as which queries are repeated most frequently.</span></span> <span data-ttu-id="b2fea-195">Por exemplo, no Microsoft SQL Server, a exibição de gerenciamento `sys.dm_exec_query_stats` tem informações estatísticas para consultas executadas recentemente.</span><span class="sxs-lookup"><span data-stu-id="b2fea-195">For example, in Microsoft SQL Server, the `sys.dm_exec_query_stats` management view has statistical information for recently executed queries.</span></span> <span data-ttu-id="b2fea-196">O texto de cada consulta está disponível na exibição `sys.dm_exec-query_plan`.</span><span class="sxs-lookup"><span data-stu-id="b2fea-196">The text for each query is available in the `sys.dm_exec-query_plan` view.</span></span> <span data-ttu-id="b2fea-197">Você pode usar uma ferramenta como o SQL Server Management Studio para executar a seguinte consulta SQL e determinar a frequência com a qual as consultas são executadas.</span><span class="sxs-lookup"><span data-stu-id="b2fea-197">You can use a tool such as SQL Server Management Studio to run the following SQL query and determine how frequently queries are performed.</span></span>

```SQL
SELECT UseCounts, Text, Query_Plan
FROM sys.dm_exec_cached_plans
CROSS APPLY sys.dm_exec_sql_text(plan_handle)
CROSS APPLY sys.dm_exec_query_plan(plan_handle)
```

<span data-ttu-id="b2fea-198">A coluna `UseCount` nos resultados indica a frequência com a qual cada consulta é executada.</span><span class="sxs-lookup"><span data-stu-id="b2fea-198">The `UseCount` column in the results indicates how frequently each query is run.</span></span> <span data-ttu-id="b2fea-199">A imagem a seguir mostra que a terceira consulta foi executada mais de 250.000 vezes, significativamente mais do que qualquer outra consulta.</span><span class="sxs-lookup"><span data-stu-id="b2fea-199">The following image shows that the third query was run more than 250,000 times, significantly more than any other query.</span></span>

![Resultados da consulta das exibições de gerenciamento dinâmico no Servidor de Gerenciamento do SQL Server][Dynamic-Management-Views]

<span data-ttu-id="b2fea-201">Aqui está a consulta SQL que está causando tantas solicitações de banco de dados:</span><span class="sxs-lookup"><span data-stu-id="b2fea-201">Here is the SQL query that is causing so many database requests:</span></span> 

```SQL
(@p__linq__0 int)SELECT TOP (2)
[Extent1].[BusinessEntityId] AS [BusinessEntityId],
[Extent1].[FirstName] AS [FirstName],
[Extent1].[LastName] AS [LastName]
FROM [Person].[Person] AS [Extent1]
WHERE [Extent1].[BusinessEntityId] = @p__linq__0
```

<span data-ttu-id="b2fea-202">Esta é a consulta que o Entity Framework gera no método `GetByIdAsync` mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b2fea-202">This is the query that Entity Framework generates in `GetByIdAsync` method shown earlier.</span></span>

### <a name="implement-the-solution-and-verify-the-result"></a><span data-ttu-id="b2fea-203">Implementar a solução e verificar o resultado</span><span class="sxs-lookup"><span data-stu-id="b2fea-203">Implement the solution and verify the result</span></span>

<span data-ttu-id="b2fea-204">Depois que você incorporar um cache, repita os testes de carga e compare os resultados para os testes de carga anteriores sem um cache.</span><span class="sxs-lookup"><span data-stu-id="b2fea-204">After you incorporate a cache, repeat the load tests and compare the results to the earlier load tests without a cache.</span></span> <span data-ttu-id="b2fea-205">Aqui estão os resultados do teste de carga após a adição de um cache ao aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="b2fea-205">Here are the load test results after adding a cache to the sample application.</span></span>

![Resultados do teste de carga de desempenho para o cenário com cache][Performance-Load-Test-Results-Cached]

<span data-ttu-id="b2fea-207">O volume de testes bem-sucedido ainda atinge um limite, mas em uma carga de usuário maior.</span><span class="sxs-lookup"><span data-stu-id="b2fea-207">The volume of successful tests still reaches a plateau, but at a higher user load.</span></span> <span data-ttu-id="b2fea-208">A taxa de solicitação com essa carga é significativamente maior do que a anterior.</span><span class="sxs-lookup"><span data-stu-id="b2fea-208">The request rate at this load is significantly higher than earlier.</span></span> <span data-ttu-id="b2fea-209">O tempo médio de teste ainda aumenta com a carga, mas o tempo de resposta máximo é 0,05 ms, comparados com 1 ms anterior a um aperfeiçoamento &mdash; 20&times;.</span><span class="sxs-lookup"><span data-stu-id="b2fea-209">Average test time still increases with load, but the maximum response time is 0.05 ms, compared with 1ms earlier &mdash; a 20&times; improvement.</span></span> 

## <a name="related-resources"></a><span data-ttu-id="b2fea-210">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="b2fea-210">Related resources</span></span>

- <span data-ttu-id="b2fea-211">[Práticas recomendadas de implementação de API][api-implementation]</span><span class="sxs-lookup"><span data-stu-id="b2fea-211">[API implementation best practices][api-implementation]</span></span>
- <span data-ttu-id="b2fea-212">[Padrão Cache-Aside][cache-aside-pattern]</span><span class="sxs-lookup"><span data-stu-id="b2fea-212">[Cache-Aside Pattern][cache-aside-pattern]</span></span>
- <span data-ttu-id="b2fea-213">[Práticas recomendadas do cache][caching-guidance]</span><span class="sxs-lookup"><span data-stu-id="b2fea-213">[Caching best practices][caching-guidance]</span></span>
- <span data-ttu-id="b2fea-214">[Padrão de Disjuntor][circuit-breaker]</span><span class="sxs-lookup"><span data-stu-id="b2fea-214">[Circuit Breaker pattern][circuit-breaker]</span></span>

[sample-app]: https://github.com/mspnp/performance-optimization/tree/master/NoCaching
[cache-aside-pattern]: /azure/architecture/patterns/cache-aside
[caching-guidance]: ../../best-practices/caching.md
[circuit-breaker]: ../../patterns/circuit-breaker.md
[api-implementation]: ../../best-practices/api-implementation.md#considerations-for-optimizing-client-side-data-access
[NewRelic]: http://newrelic.com/azure
[NewRelic-server-requests]: _images/New-Relic.jpg
[Performance-Load-Test-Results-Uncached]:_images/InitialLoadTestResults.jpg
[Dynamic-Management-Views]: _images/SQLServerManagementStudio.jpg
[Performance-Load-Test-Results-Cached]: _images/CachedLoadTestResults.jpg