---
title: Principais problemas do Dispatcher
seo-title: Top issues for AEM Dispatcher
description: Principais problemas do AEM Dispatcher
seo-description: Top issues for Adobe AEM Dispatcher
exl-id: 4dcc7318-aba5-4b17-8cf4-190ffefbba75
source-git-commit: 3a0e237278079a3885e527d7f86989f8ac91e09d
workflow-type: ht
source-wordcount: '1633'
ht-degree: 100%

---

# Perguntas frequentes sobre os principais problemas do AEM Dispatcher

![Configuração do Dispatcher](assets/CQDispatcher_workflow_v2.png)

## Introdução

### O que é o Dispatcher?

O Dispatcher é uma ferramenta de armazenamento em cache e/ou balanceamento de carga do Adobe Experience Manager que ajuda a possibilitar um ambiente de criação da Web rápido e dinâmico. Para armazenamento em cache, o Dispatcher funciona como parte de um servidor HTTP, como o Apache, com o objetivo de armazenar (ou &quot;armazenar em cache&quot;) o máximo possível do conteúdo estático do site e acessar o mecanismo de layout do site com a menor frequência possível. Em uma função de balanceamento de carga, o Dispatcher distribui solicitações de usuário (carga) em diferentes instâncias do AEM (renderizações).

Para armazenamento em cache, o módulo Dispatcher usa a capacidade do servidor Web de fornecer conteúdo estático. O Dispatcher coloca os documentos em cache na raiz do documento do servidor web.

### Como o Dispatcher executa o armazenamento em cache?

O Dispatcher usa a capacidade do servidor Web de fornecer conteúdo estático. Ele armazena documentos em cache na raiz do documento do servidor Web. O Dispatcher tem dois métodos primários para atualizar o conteúdo de cache quando mudanças forem feitas ao website.

* **As Atualizações de conteúdo** removem as páginas que foram alteradas, bem como os arquivos que estão diretamente associados a elas.
* **A invalidação automática** invalida automaticamente as partes do cache que podem estar desatualizadas após uma atualização. Por exemplo, ela marca de forma eficaz as páginas relevantes como desatualizadas, sem excluir nada.

### Quais são os benefícios do balanceamento de carga?

O balanceamento de carga distribui solicitações do usuário (carga) em várias instâncias do AEM. A lista a seguir descreve as vantagens do balanceamento de carga:

* **Maior poder de processamento**: na prática, significa que o Dispatcher compartilha solicitações de documento entre várias instâncias do AEM. Como cada instância tem menos documentos para processar, você tem tempos de resposta mais rápidos. O Dispatcher mantém estatísticas internas para cada categoria de documento, de modo que ele possa estimar o carregamento e distribuir as consultas com eficiência.
* **Maior cobertura à prova de falhas**: se o Dispatcher não receber respostas de uma instância, ele retornará automaticamente solicitações para uma outra ou uma das outras instâncias. Assim, se uma instância se tornar indisponível, o único efeito é uma desaceleração do site, proporcional à potência computacional perdida.

>[!NOTE]
>
>Para obter mais detalhes, consulte a [página de Visão geral do Dispatcher](dispatcher.md)

## Instalação e configuração

### De onde baixar o módulo Dispatcher?

Você pode baixar o módulo Dispatcher mais recente na página [Notas de versão do Dispatcher](release-notes.md).

### Como instalar o módulo Dispatcher?

Consulte a página [Instalação do Dispatcher](dispatcher-install.md).

### Como configurar o módulo Dispatcher?

Consulte a página [Configuração do Dispatcher](dispatcher-configuration.md).

### Como configurar o Dispatcher para a instância do autor?

Consulte [Uso do Dispatcher com uma instância de autor](dispatcher.md#using-a-dispatcher-with-an-author-server) para obter as etapas detalhadas.

### Como configurar o Dispatcher com vários domínios?

Você pode configurar o Dispatcher CQ com vários domínios, desde que os domínios satisfaçam às seguintes condições:

* O conteúdo da Web de ambos os domínios deve ser armazenado em um único repositório do AEM
* Os arquivos no cache do Dispatcher podem ser invalidados separadamente para cada domínio

Leia [Uso do Dispatcher com vários domínios](dispatcher-domains.md) para obter mais detalhes.

### Como configurar o Dispatcher, de modo que todas as solicitações de um usuário sejam roteadas para a mesma instância de publicação?

Você pode usar o recurso [conexões aderentes](dispatcher-configuration.md#identifying-a-sticky-connection-folder-stickyconnectionsfor), que garante que todos os documentos de um usuário sejam processados na mesma instância do AEM. Esse recurso é importante se você usar páginas personalizadas e dados de sessão. Os dados são armazenados na instância. Portanto, as solicitações subsequentes do mesmo usuário devem retornar a essa instância ou os dados serão perdidos.

Como as conexões aderentes restringem a capacidade do Dispatcher de otimizar as solicitações, você deve usar essa abordagem somente quando necessário. Você pode especificar a pasta que contém os documentos &quot;fixos&quot;, garantindo que todos os documentos dessa pasta sejam processados na mesma instância para um usuário.

### Posso usar conexões aderentes e armazenamento em cache em conjunto?

Para a maioria das páginas que usam conexões aderentes, você deve desativar o armazenamento em cache. Caso contrário, a mesma instância da página será exibida para todos os usuários, independentemente do conteúdo da sessão.

Para alguns aplicativos, pode ser possível usar conexões aderentes e armazenamento em cache. Por exemplo, se você exibir um formulário que grava dados em uma sessão, será possível usar conexões aderentes e armazenamento em cache em conjunto.

### Um Dispatcher e uma instância de publicação do AEM podem residir no mesmo computador?

Sim, se o computador for suficientemente eficiente. No entanto, é recomendável configurar o Dispatcher e a instância de publicação do AEM em computadores diferentes.

Normalmente, a instância de publicação reside dentro do firewall, e o Dispatcher reside no DMZ. Se decidir manter a instância de publicação e o Dispatcher no mesmo computador, verifique se as configurações de firewall proíbem o acesso direto à instância de publicação por meio de redes externas.

### Posso armazenar em cache somente arquivos com extensões específicas?

Sim. Por exemplo, se você quiser armazenar em cache somente arquivos GIF, especifique *.gif, na seção de cache do arquivo de configuração dispatcher.any.

### Como excluir arquivos do cache?

Você pode excluir arquivos do cache usando uma solicitação HTTP. Quando a solicitação HTTP é recebida, o Dispatcher exclui os arquivos do cache. O Dispatcher armazena os arquivos em cache novamente somente quando recebe uma solicitação do cliente para a página. A exclusão de arquivos em cache dessa maneira é adequada para sites que provavelmente não receberão solicitações simultâneas para a mesma página.

A solicitação HTTP tem a seguinte sintaxe:

```
POST /dispatcher/invalidate.cache HTTP/1.1
CQ-Action: Activate
CQ-Handle: path-pattern
Content-Length: 0
```

O Dispatcher exclui os arquivos e pastas em cache que têm nomes que correspondem ao valor do cabeçalho CQ-Handle. Por exemplo, um CQ-Handle de `/content/geomtrixx-outdoors/en` corresponde aos seguintes itens:

Todos os arquivos (de qualquer extensão de arquivo) nomeados com en no diretório geometrixx-outdoors;
qualquer diretório com nome `_jcr_content` abaixo do diretório en (que, se existir, contém renderizações em cache de subnós da página).
O diretório en só será excluído se `CQ-Action` for `Delete` ou `Deactivate`.

Para obter mais detalhes sobre este tópico, consulte [Invalidação manual do cache do Dispatcher](page-invalidate.md).

### Como implementar o armazenamento em cache sensível a permissões?

Consulte a página [Armazenamento em cache de conteúdo seguro](permissions-cache.md).

### Como proteger as comunicações entre as instâncias do Dispatcher e do CQ?

Consulte as páginas [Lista de verificação de segurança do Dispatcher](security-checklist.md) e [Lista de verificação de segurança do AEM](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security-checklist.html?lang=pt-BR).

### Problema do Dispatcher `jcr:content` alterado para `jcr%3acontent`

**Pergunta**: Recentemente, enfrentamos um problema no nível do dispatcher, onde uma das chamadas ajax que estava recebendo dados do repositório CQ tinha `jcr:content` nele e foi codificada para `jcr%3acontent`, o que gerou um conjunto de resultados incorreto.

**Resposta**: Use o método `ResourceResolver.map()` para obter um URL “amigável” que possa ser usado/emitido para solicitações de resolução de problemas de armazenamento em cache com o Dispatcher. O método map() codifica dois pontos `:` para sublinhados, e o método resolve() os decodifica para o formato legível SLING JCR. Você precisa usar o método map() para gerar o URL usado na chamada de Ajax.

Leia mais: [https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#namespace-mangling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#namespace-mangling)

## Limpar o Dispatcher

### Como configurar agentes de limpeza do Dispatcher em uma instância de publicação?

Consulte a página [Replicação](https://helpx.adobe.com/content/help/br/experience-manager/6-4/sites/deploying/using/replication.html#ConfiguringyourReplicationAgents).

### Como solucionar problemas de limpeza do Dispatcher?

[Consulte este artigo de solução de problemas](https://helpx.adobe.com/content/help/br/experience-manager/kb/troubleshooting-dispatcher-flushing-issues.html) que responde às seguintes perguntas:

* Como devo depurar uma situação em que nenhum conteúdo está sendo salvo no cache do Dispatcher?
* Como depurar uma situação em que os arquivos de cache não estão sendo atualizados?
* Como depurar uma situação em que nada relacionado à limpeza do Dispatcher está funcionando?

Se as operações de exclusão estiverem fazendo com que o Dispatcher libere, [use a solução alternativa nesta publicação de blog da comunidade, por Sensei Martin](https://mkalugin-cq.blogspot.in/2012/04/i-have-been-working-on-following.html).

### Como liberar ativos DAM do cache do Dispatcher?

Você pode usar o recurso &quot;replicação em cadeia&quot;. Com esse recurso ativado, o agente de limpeza do dispatcher envia uma solicitação de liberação quando uma replicação é recebida do autor.

Para habilitá-lo:

1. [Siga as etapas aqui](page-invalidate.md#invalidating-dispatcher-cache-from-a-publishing-instance) para criar agentes de liberação ao publicar
1. Vá para a configuração de cada um desses agentes e, na guia **Acionadores**, marque a caixa **Em recebimento**.

## Diversos

Como o Dispatcher determina se um documento está atualizado?
Para determinar se um documento está atualizado, o Dispatcher executa estas ações:

Verifica se o documento está sujeito a invalidação automática. Caso contrário, o documento será considerado atualizado.
Se o documento estiver configurado para invalidação automática, o Dispatcher verificará se ele é mais antigo ou mais recente do que a última alteração disponível. Se for mais antigo, o Dispatcher solicitará a versão atual da instância do AEM e substituirá a versão no cache.

### Como o Dispatcher retorna documentos?

Você pode definir se o Dispatcher armazena em cache um documento usando o arquivo [Configuração do Dispatcher](dispatcher-configuration.md), `dispatcher.any`. O Dispatcher verifica a solicitação em relação à lista de documentos que podem ser armazenados em cache. Se o documento não estiver nessa lista, o Dispatcher solicitará o documento da instância do AEM.

A propriedade `/rules` controla quais documentos são armazenados em cache de acordo com o caminho do documento. Independentemente da propriedade `/rules`, o Dispatcher nunca armazena em cache um documento nas seguintes circunstâncias:

* Se o URI da solicitação contiver um ponto de interrogação `(?)`.
* Isso geralmente indica uma página dinâmica, como um resultado de pesquisa, que não precisa ser armazenado em cache.
* A extensão do arquivo está ausente.
* O servidor Web precisa da extensão para determinar o tipo de documento (o tipo MIME).
* O cabeçalho de autenticação está definido (isso pode ser configurado)
* Se a instância do AEM responder com os seguintes cabeçalhos:
   * no-cache
   * no-store
   * must-revalidate

O Dispatcher armazena os arquivos em cache no servidor Web como se fossem parte de um site estático. Se um usuário solicitar um documento armazenado em cache, o Dispatcher verificará se esse documento existe no sistema de arquivos do servidor Web. Nesse caso, o Dispatcher retornará os documentos. Se não estiver em cache, o Dispatcher solicitará o documento da instância do AEM.

>[!NOTE]
>
>Os métodos GET ou HEAD (para o cabeçalho HTTP) podem ser armazenados em cache pelo Dispatcher. Para obter informações adicionais sobre o armazenamento em cache do cabeçalho de resposta, consulte a seção [Armazenamento em cache de cabeçalhos de resposta HTTP](dispatcher-configuration.md#caching-http-response-headers).

### Posso implementar vários Dispatchers em uma configuração?

Sim. Nesses casos, verifique se os Dispatchers podem acessar o site do AEM diretamente. Um Dispatcher não pode lidar com solicitações provenientes de outro Dispatcher.
