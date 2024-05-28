---
title: Configuração do Adobe Experience Manager Dispatcher para evitar ataques CSRF
description: Saiba como configurar o Adobe Experience Manager Dispatcher para impedir ataques de falsificações de solicitações entre sites.
topic-tags: dispatcher
content-type: reference
exl-id: bcd38878-f977-46a6-b01a-03e4d90aef01
source-git-commit: 0a1aa854ea286a30c3527be8fc7c0998726a663f
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 48%

---

# Configuração do Adobe Experience Manager Dispatcher para evitar ataques CSRF{#configuring-dispatcher-to-prevent-csrf-attacks}

O AEM (Adobe Experience Manager) fornece uma estrutura destinada a impedir ataques de falsificação de solicitação entre sites. Para usar adequadamente essa estrutura, faça as seguintes alterações na configuração do Dispatcher:

>[!NOTE]
>
>Atualize os números de regras nos exemplos abaixo com base em sua configuração existente. Lembre-se de que os Dispatchers usam a última regra correspondente para conceder ou negar uma permissão. Portanto, coloque as regras perto da parte inferior da lista existente.

1. Na seção `/clientheaders` de `author-farm.any` e `publish-farm.any`, adicione a seguinte entrada no final da lista:\
   `CSRF-Token`
1. Na seção /filters do arquivo `author-farm.any` e `publish-farm.any` ou `publish-filters.any`, adicione a linha a seguir para permitir solicitações de `/libs/granite/csrf/token.json` por meio do Dispatcher.\
   `/0999 { /type "allow" /glob " * /libs/granite/csrf/token.json*" }`

1. Na seção `/cache /rules` de `publish-farm.any`, adicione uma regra para impedir que o Dispatcher armazene em cache o arquivo `token.json`. Geralmente, os autores ignoram o armazenamento em cache, de modo que não é necessário adicionar a regra ao `author-farm.any`.

   `/0999 { /glob "/libs/granite/csrf/token.json" /type "deny" }`

Para validar se a configuração está funcionando, observe o dispatcher.log no modo DEBUG. Isso pode ajudá-lo a validar que o `token.json` para garantir que não seja armazenado em cache ou bloqueado por filtros. Você deve ver mensagens semelhantes a:\
`... checking [/libs/granite/csrf/token.json]  `
`... request URL not in cache rules: /libs/granite/csrf/token.json`\
`... cache-action for [/libs/granite/csrf/token.json]: NONE`

Você também pode confirmar se as solicitações foram bem-sucedidas no `access_log` do Apache. As solicitações para ``/libs/granite/csrf/token.json devem retornar um código de status HTTP 200.
