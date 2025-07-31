---
title: Resolução de problemas do Dispatcher
description: Saiba como solucionar problemas do Dispatcher.
cmgrlastmodified: 01.11.2007 08 22 29 [aheimoz]
pageversionid: 1193211344162
template: /apps/docs/templates/contentpage
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
exl-id: 29f338ab-5d25-48a4-9309-058e0cc94cff
source-git-commit: c41b4026a64f9c90318e12de5397eb4c116056d9
workflow-type: ht
source-wordcount: '472'
ht-degree: 100%

---

# Resolução de problemas do Dispatcher {#troubleshooting-dispatcher-problems}

>[!NOTE]
>
>As versões do Dispatcher são independentes do AEM. No entanto, a documentação do Dispatcher está incorporada na documentação do AEM. Sempre use a documentação do Dispatcher incorporada à documentação da versão mais recente do AEM.
>
>Você pode ter sido redirecionado para esta página se clicou em um link para a documentação do Dispatcher. Esse link está incorporado na documentação de uma versão anterior do AEM.

>[!NOTE]
>
>Consulte a <!-- URL is 404[Dispatcher Knowledge Base](https://helpx.adobe.com/experience-manager/kb/index/dispatcher.html), -->[Resolução de problemas de limpeza do Dispatcher](https://experienceleague.adobe.com/search.html?lang=br#q=troubleshooting%20dispatcher%20flushing%20issues&sort=relevancy&f:el_product=[Experience%20Manager]) e as [perguntas frequentes sobre os principais problemas do Dispatcher](dispatcher-faq.md) para mais informações.

## Verificar a configuração básica {#check-the-basic-configuration}

Como sempre, as primeiras etapas são verificar as noções básicas:

* [Confirmar operação básica](/help/using/dispatcher-configuration.md#confirming-basic-operation)
* Verificar todos os arquivos de log do servidor da Web e do Dispatcher. Se necessário, aumente o `loglevel` usado para o [log](/help/using/dispatcher-configuration.md#logging) do Dispatcher.

* [Verifique sua configuração](/help/using/dispatcher-configuration.md):

   * Você tem vários Dispatchers?

      * Você determinou qual Dispatcher está lidando com o site/página que você está investigando?

   * Você implementou filtros?

      * Esses filtros estão afetando o assunto que você está investigando?

## Ferramentas de diagnóstico do IIS {#iis-diagnostic-tools}

O IIS fornece várias ferramentas de rastreamento, dependendo da versão real:

* IIS 6 - ferramentas de diagnóstico do IIS podem ser baixadas e configuradas
* IIS 7 - o rastreamento é totalmente integrado

Essas ferramentas podem ajudar você a monitorar a atividade.

<!-- Both URLs in this topic 404! >
## IIS and 404 Not Found {#iis-and-not-found}

When using IIS, you might experience `404 Not Found` being returned in various scenarios. If so, see the following Knowledge Base articles.

* [IIS 6/7: POST method returns 404](https://helpx.adobe.com/experience-manager/kb/IIS6IsapiFilters.html)
* [IIS 6: Requests to URLs that contain the base path `/bin` return a `404 Not Found`](https://helpx.adobe.com/experience-manager/kb/RequestsToBinDirectoryFailInIIS6.html)

Also check that the Dispatcher cache root and the IIS document root are set to the same directory. -->

## Problemas ao excluir modelos de fluxo de trabalho {#problems-deleting-workflow-models}

**Sintomas**

Problemas ao tentar excluir modelos de fluxos de trabalho ao acessar uma instância de autor do AEM por meio do Dispatcher.

**Etapas a serem reproduzidas:**

1. Faça logon na sua instância do autor (confirme se as solicitações estão sendo roteadas pelo Dispatcher).
1. Crie um fluxo de trabalho; por exemplo, com o título definido como workflowToDelete.
1. Confirme se o fluxo de trabalho foi criado com êxito.
1. Selecione e clique com o botão direito do mouse no fluxo de trabalho e clique em **Excluir**.

1. Clique em **Sim** para confirmar.
1. Uma caixa de mensagem de erro é exibida mostrando o seguinte:\
   `ERROR 'Could not delete workflow model!!`.

**Resolução**

Adicione os seguintes cabeçalhos à seção `/clientheaders` do arquivo `dispatcher.any`:

* `x-http-method-override`
* `x-requested-with`

```
{  
{  
/clientheaders  
{  
...  
"x-http-method-override"  
"x-requested-with"  
}
```

## Interferência com mod_dir (Apache) {#interference-with-mod-dir-apache}

Esse processo descreve como o Dispatcher interage com o `mod_dir` no servidor da Web Apache, pois isso pode levar a vários efeitos possivelmente inesperados:

### Apache 1.3 {#apache}

No Apache 1.3, `mod_dir` lida com cada solicitação em que o URL mapeia para um diretório no sistema de arquivos.

Ele irá:

* redirecionar a solicitação para um arquivo `index.html` existente
* gerar uma listagem de diretórios

Quando o Dispatcher estiver ativado, ele processará essas solicitações se registrando como um manipulador para o tipo de conteúdo `httpd/unix-directory`.

### Apache 2.x {#apache-x}

No Apache 2.x, isso é diferente. Um módulo pode lidar com diferentes estágios da solicitação, como correção de URL. O `mod_dir` lida com esse estágio redirecionando uma solicitação (quando o URL mapeia para um diretório) para o URL com uma `/` anexada.

O Dispatcher não intercepta a correção `mod_dir`, mas lida completamente com a solicitação para o URL redirecionado (ou seja, com `/` anexada). Esse processo pode causar um problema se o servidor remoto (por exemplo, o AEM) manipula as solicitações para `/a_path` de forma diferente das solicitações para `/a_path/` (quando `/a_path` mapeia para um diretório existente).

Se essa situação ocorrer, você deve:

* desativar o `mod_dir` para a árvore secundária `Directory` ou `Location` manipulada pelo Dispatcher

* usar `DirectorySlash Off` para configurar `mod_dir` para não anexar `/`
