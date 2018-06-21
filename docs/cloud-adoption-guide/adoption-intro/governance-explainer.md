---
title: 'Explicação: o que é a governança de nuvem?'
description: Explica o conceito de governança de recursos para o Azure e a nuvem
author: petertay
ms.openlocfilehash: 3404beaa719177ee7638feed8a8442b5c3b455c6
ms.sourcegitcommit: 26b04f138a860979aea5d253ba7fecffc654841e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36206473"
---
# <a name="explainer-what-is-cloud-resource-governance"></a>Explicação: o que é a governança dos recursos da nuvem?

Na explicação [Como funciona o Azure?](azure-explainer.md), você aprendeu que o Azure é uma coleção de servidores e hardware de rede que executa software e hardware virtualizado em nome dos usuários. Azure permite o desenvolvimento de sua organização e os departamentos de TI para ser ágil, tornando mais fácil criar, ler, atualizar e excluir recursos conforme necessário.

No entanto, enquanto irrestrito fornecendo acesso a recursos para desenvolvedores pode torná-los muito ágil, também poderá causar consequências não intencionais de custo. Por exemplo, uma equipe de desenvolvimento pode ser aprovada para implantar um conjunto de recursos para teste, mas esquecer de excluí-las quando o teste for concluído. Esses recursos continuarão a gerar custos, embora seu uso não seja mais necessário ou aprovado. 

A solução para esse problema é a **governança** de acesso a recursos. Governança refere-se ao processo contínuo de gerenciamento, monitoramento e auditoria, o uso de recursos do Azure para atender as metas e requisitos da sua organização. 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ii94] 

Essas metas e requisitos são exclusivos de cada organização e, portanto, não é possível ter uma abordagem única para governança. Em vez disso, o Azure implementa duas ferramentas de administração primária, **recursos com base em controle de acesso (RBAC)**, e **diretiva recursos**, e depende de cada organização para criar seu modelo de governança usá-los.

O RBAC define funções, e as funções definem os recursos para um usuário atribuído à função. Por exemplo, a função **proprietário** habilita todos os recursos (criar, ler, atualizar e excluir) para um recurso, enquanto as funções **leitor** habilita somente a capacidade de leitura. As funções podem ser definidas com um escopo amplo que se aplique a vários tipos de recursos ou um escopo estreito que se aplique a alguns. 

As políticas de recursos definem regras para a criação de recursos. Por exemplo, uma política de recursos pode limitar o SKU de uma VM para um tamanho pré-aprovado específico. Ou então, uma política de recursos pode impor a adição de uma marca com um centro de custo quando a solicitação é feita para criar o recurso. 

Ao configurar essas ferramentas, uma consideração importante é o balanceamento da governança versus agilidade organizacional. Ou seja, quanto mais restritiva a política de governança, menos ágeis se tornarão seus desenvolvedores e profissionais de TI. Isso ocorre porque uma política restritiva governança pode exigir mais etapas manuais, como exigir um desenvolvedor preencher um formulário ou enviar um email para uma pessoa da equipe de governança para criar manualmente um recurso. A equipe de governança possui recursos finitos e pode ficar acumulada, resultando em equipes de desenvolvimento improdutivos aguardando seus recursos para recursos desnecessários e criados acúmulo de custos enquanto esperam a ser excluído.

## <a name="next-steps"></a>Próximas etapas

A próxima etapa na adoção do Azure é [entender a identidade digital no Azure](tenant-explainer.md) e [criar seu primeiro usuário no Azure AD][docs-add-users-to-aad].

<!-- Links -->

[docs-add-users-to-aad]: /azure/active-directory/add-users-azure-active-directory?toc=/azure/architecture/cloud-adoption-guide/toc.json