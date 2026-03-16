---
title: Aprimoramento do Dispatcher ETag para revalidação de CDN
description: Disponibilidade, status de suporte e comportamento de INTERNAL_AEM_DISPATCHER_ETAG_ENHANCEMENT no AEM as a Cloud Service.
source-git-commit: ac0fafd060643903735ff565072ef2c5bee970be
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---

# Aprimoramento do Dispatcher ETag para revalidação de CDN

## Visão geral

O sinalizador `INTERNAL_AEM_DISPATCHER_ETAG_ENHANCEMENT` permite que o Dispatcher avalie o cabeçalho de solicitação `If-None-Match` em ocorrências de cache. Quando o valor de entrada `If-None-Match` corresponder ao `ETag` armazenado em cache, o Dispatcher poderá retornar `304 Not Modified` em vez de `200 OK`.

Esse comportamento foi projetado para reduzir transferências de carga desnecessárias entre o CDN e o Dispatcher e aumentar a eficiência do cache condicional.

## Disponibilidade

- Versão do Dispatcher: `2.0.264`
- Compilação do AEM SDK: `aem-sdk-2026.2.24464.20260214T050318Z-260100`

## Suporte ao AEM as a Cloud Service

No AEM as a Cloud Service, esse recurso é compatível com o uso do cliente.

Os clientes podem habilitá-lo definindo a variável de ambiente `INTERNAL_AEM_DISPATCHER_ETAG_ENHANCEMENT` no Cloud Manager. A Adobe também pode ativá-la em nome do cliente quando necessário.

Quando habilitado, e quando o CDN envia `If-None-Match` e o `ETag` relevante está presente no cache do Dispatcher, taxas de resposta `304` mais altas entre o CDN e o Dispatcher são esperadas. Esse aumento é o resultado esperado.

## Exemplo de configuração (cabeçalho ETag de cache)

Para que esse aprimoramento seja eficaz, verifique se o Dispatcher armazena em cache o cabeçalho de resposta do `ETag` e se o servidor da Web está configurado para evitar a geração de ETags baseados no sistema de arquivos.

Exemplo de seção de cache `dispatcher.any`:

```text
/cache {
  /headers {
    "Cache-Control"
    "Content-Type"
    "Expires"
    "Last-Modified"
    "ETag"
  }
}
```

Exemplo de diretiva do Apache no contexto do Dispatcher vhost:

```apache
FileETag none
```

Para obter orientação sobre o armazenamento em cache do cabeçalho da linha de base, consulte [Armazenamento em cache de cabeçalhos de resposta HTTP](dispatcher-configuration.md#caching-http-response-headers).

## Exemplo de validação

Depois de habilitar a variável de ambiente e implantar alterações de configuração:

1. Solicite uma vez para aquecer o cache e capturar o `ETag` retornado.
1. Solicitar novamente com `If-None-Match: <etag-value>`.
1. Confirmar se o Dispatcher retorna `304 Not Modified` para fluxos de revalidação de ocorrência de cache.

## Referência pública (comportamento relacionado)

Para obter orientação de linha de base voltada para o cliente sobre o armazenamento em cache do cabeçalho e manuseio de `ETag` no Dispatcher, consulte:

- [Configurar o Dispatcher - Armazenamento em cache de cabeçalhos de resposta HTTP](https://experienceleague.adobe.com/pt-br/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration#caching-http-response-headers)

&quot;Este recurso está disponível no Dispatcher `2.0.264` (AEM SDK `2026.2.24464`). Quando habilitado, o Dispatcher pode validar `If-None-Match` em relação aos valores de `ETag` armazenados em cache e retornar `304 Not Modified` em ocorrências de cache. No AEM as a Cloud Service, isso é suportado e pode ser ativado por meio da configuração do ambiente Cloud Manager.&quot;
