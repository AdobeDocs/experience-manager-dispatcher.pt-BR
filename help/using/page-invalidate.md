---
title: Invalidar páginas em cache do AEM
description: Saiba como configurar a interação entre o Dispatcher e o AEM para garantir um gerenciamento eficiente do cache.
cmgrlastmodified: 01.11.2007 08 22 29 [aheimoz]
pageversionid: 1193211344162
template: /apps/docs/templates/contentpage
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
exl-id: 90eb6a78-e867-456d-b1cf-f62f49c91851
source-git-commit: c41b4026a64f9c90318e12de5397eb4c116056d9
workflow-type: ht
source-wordcount: '1407'
ht-degree: 100%

---

# Invalidar páginas em cache do AEM {#invalidating-cached-pages-from-aem}

Ao usar o Dispatcher com o AEM, a interação deve ser configurada para garantir um gerenciamento eficiente do cache. Dependendo do seu ambiente, a configuração também pode aumentar o desempenho.

## Configurar contas de usuários do AEM {#setting-up-aem-user-accounts}

A conta de usuário `admin` padrão é usada para autenticar os agentes de replicação instalados por padrão. Crie uma conta de usuário dedicada para uso com agentes de replicação.

Para mais informações, consulte a seção [Configurar usuários de replicação e transporte](https://experienceleague.adobe.com/pt-br/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions#VerificationSteps) da lista de verificação de segurança do AEM.

<!-- OLD URL from above https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html#VerificationSteps -->

## Invalidar o cache do Dispatcher no ambiente de criação {#invalidating-dispatcher-cache-from-the-authoring-environment}

Um agente de replicação na instância do autor do AEM envia uma solicitação de invalidação de cache para o Dispatcher quando uma página é publicada. O Dispatcher atualiza o arquivo no cache conforme o novo conteúdo é publicado.

<!-- 

Comment Type: draft

<p>Cache invalidation requests for a page are also generated for any aliases or vanity URLs that are configured in the page properties. </p>

 -->

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-0436B4A35714BFF67F000101@AdobeID)
Last Modified Date: 2017-05-25T10:37:23.679-0400

<p>Hiding this information due to <a href="https://jira.corp.adobe.com/browse/CQDOC-5805">CQDOC-5805</a>.</p>

 -->

Use o procedimento a seguir para configurar um agente de replicação na instância de criação do AEM. A configuração invalida o cache do Dispatcher após a ativação da página:

1. Abra o console Ferramentas do AEM. (`https://localhost:4502/miscadmin#/etc`)
1. Abra o agente de replicação necessário abaixo de Ferramentas/Replicação/Agentes no autor. Você pode usar o agente de limpeza do Dispatcher instalado por padrão.
1. Clique em Editar e, na guia Configurações, certifique-se de que **Ativado** esteja selecionado.

1. (Opcional) Para ativar solicitações de invalidação de alias ou caminho personalizado, selecione a opção **Atualização de alias**.
1. Na guia Transporte, acesse o Dispatcher inserindo o URI.

   Se estiver usando o agente de liberação padrão do Dispatcher, atualize o nome do host e a porta, por exemplo: https://&lt;*dispatcherHost*>:&lt;*portApache*>/dispatcher/invalidate.cache

   **Observação:** para agentes de liberação do Dispatcher, a propriedade URI será usada somente se você usar entradas de host virtual baseadas em caminho para diferenciar farms. Use esse campo para direcionar o farm a ser invalidado. Por exemplo, o farm nº 1 tem um host virtual de `www.mysite.com/path1/*`, e o farm nº 2 tem um host virtual de `www.mysite.com/path2/*`. Você pode usar um URL de `/path1/invalidate.cache` para direcionar o primeiro farm, e `/path2/invalidate.cache` para direcionar o segundo farm. Para obter mais informações, consulte [Uso do Dispatcher com vários domínios](dispatcher-domains.md).

1. Configure outros parâmetros conforme necessário.
1. Clique em OK para ativar o agente.

Como alternativa, você também pode acessar e configurar o agente de limpeza do Dispatcher direto da interface do [AEM Touch](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/deploying/configuring/replication#configuring-a-dispatcher-flush-agent).

Para obter detalhes adicionais sobre como habilitar o acesso a URLs personalizados, consulte [Habilitar o acesso a URLs personalizados](dispatcher-configuration.md#enabling-access-to-vanity-urls-vanity-urls).

>[!NOTE]
>
>O agente para limpar o cache do Dispatcher não precisa ter um nome de usuário e senha, mas, se configurado, eles serão enviados com um processo de autenticação básica.

Há dois problemas potenciais com essa abordagem:

* O Dispatcher deve estar acessível na instância de criação. Se sua rede (por exemplo, o firewall) estiver configurada de modo que o acesso entre os dois seja restrito, esse pode não ser o caso.

* A publicação e a invalidação do cache ocorrem simultaneamente. Dependendo do tempo, um usuário pode solicitar uma página logo após sua remoção do cache e antes da publicação da nova página. O AEM retorna a página antiga, e o Dispatcher a armazena em cache novamente. Esse problema ocorre mais com sites grandes.

## Invalidar o cache do Dispatcher de uma instância de publicação {#invalidating-dispatcher-cache-from-a-publishing-instance}

Em determinadas circunstâncias, os ganhos de desempenho podem ser feitos pela transferência do gerenciamento de cache do ambiente de criação para uma instância de publicação. Em seguida, o ambiente de publicação (não o ambiente de criação do AEM) enviará uma solicitação de invalidação de cache para o Dispatcher quando uma página publicada for recebida.

Essas circunstâncias incluem:

<!-- 

Comment Type: draft

<p>Cache invalidation requests for a page are also generated for any aliases or vanity URLs that are configured in the page properties. </p>

 -->

* Como evitar possíveis conflitos de tempo entre o Dispatcher do AEM e a instância de publicação (consulte [Invalidar o cache do Dispatcher do ambiente de criação](#invalidating-dispatcher-cache-from-the-authoring-environment)).
* O sistema inclui várias instâncias de publicação que residem em servidores de alto desempenho e somente uma instância de criação.

>[!NOTE]
>
>Uma pessoa responsável pela administração do AEM que seja experiente deve tomar a decisão de usar esse método.

Um agente de replicação que opera na instância de publicação controla a limpeza do Dispatcher. No entanto, a configuração é feita no ambiente de criação e depois transferida pela ativação do agente:

1. Abra o console Ferramentas do AEM.
1. Abra o agente de replicação necessário abaixo de Ferramentas/Replicação/Agentes na publicação. Você pode usar o agente de limpeza do Dispatcher instalado por padrão.
1. Clique em Editar e, na guia Configurações, certifique-se de que **Ativado** esteja selecionado.
1. (Opcional) Para ativar solicitações de invalidação de alias ou caminho personalizado, selecione a opção **Atualização de alias**.
1. Na guia Transporte, acesse o Dispatcher inserindo o URI necessário.\
   Se você estiver usando o agente de limpeza do Dispatcher padrão, atualize o nome do host e a porta; por exemplo, `http://<dispatcherHost>:<portApache>/dispatcher/invalidate.cache`

   **Observação:** para agentes de liberação do Dispatcher, a propriedade URI será usada somente se você usar entradas de host virtual baseadas em caminho para diferenciar farms. Use esse campo para direcionar o farm a ser invalidado. Por exemplo, o farm nº 1 tem um host virtual de `www.mysite.com/path1/*`, e o farm nº 2 tem um host virtual de `www.mysite.com/path2/*`. Você pode usar um URL de `/path1/invalidate.cache` para direcionar o primeiro farm, e `/path2/invalidate.cache` para direcionar o segundo farm. Para obter mais informações, consulte [Uso do Dispatcher com vários domínios](dispatcher-domains.md).

1. Configure outros parâmetros conforme necessário.
1. Faça logon na instância de publicação e valide a configuração do agente de liberação. Além disso, verifique se ele está ativado.
1. Repita o procedimento para cada instância de publicação afetada.

Após a configuração, ao ativar uma página do autor para publicar, esse agente inicia uma replicação padrão. O log inclui mensagens indicando solicitações provenientes do seu servidor de publicação, semelhantes ao seguinte exemplo:

1. `<publishserver> 13:29:47 127.0.0.1 POST /dispatcher/invalidate.cache 200`

## Invalidar o cache do Dispatcher manualmente {#manually-invalidating-the-dispatcher-cache}

Para invalidar (ou liberar) o cache do Dispatcher sem ativar uma página, emita uma solicitação HTTP para o Dispatcher. Por exemplo, você pode criar um aplicativo do AEM que permita aos administradores ou outros aplicativos liberar o cache.

A solicitação HTTP faz com que o Dispatcher exclua arquivos específicos do cache. Como opção, o Dispatcher atualiza o cache com uma nova cópia.

### Exclusão de arquivos em cache {#delete-cached-files}

Emita uma solicitação HTTP que faça com que o Dispatcher exclua arquivos do cache. O Dispatcher armazena os arquivos em cache novamente somente quando recebe uma solicitação do cliente para a página. A exclusão de arquivos em cache dessa maneira é adequada para sites que provavelmente não receberão solicitações simultâneas para a mesma página.

A solicitação HTTP tem o seguinte formato:

```xml
POST /dispatcher/invalidate.cache HTTP/1.1  
CQ-Action: Activate  
CQ-Handle: path-pattern
Content-Length: 0
```

O Dispatcher libera (exclui) os arquivos e pastas em cache com nomes que correspondam ao valor do cabeçalho `CQ-Handler`. Por exemplo, um `CQ-Handle` de `/content/geomtrixx-outdoors/en` corresponde aos seguintes itens:

* Todos os arquivos (de qualquer extensão de arquivo) com nome `en` no diretório `geometrixx-outdoors`

* Qualquer diretório chamado `_jcr_content` abaixo do diretório `en` (que, se existir, contém renderizações em cache de subnós da página)

Todos os outros arquivos no cache do Dispatcher (ou até um nível específico, dependendo da configuração `/statfileslevel`) são invalidados ao tocar no arquivo `.stat`. A última data de modificação desse arquivo é comparada com a última data de modificação de um documento em cache, e o documento será buscado novamente se o arquivo `.stat` for mais recente. Consulte [Invalidação de arquivos por nível de pasta](dispatcher-configuration.md#main-pars_title_26) para obter mais detalhes.

A invalidação (ou seja, tocar em arquivos .stat) pode ser evitada por enviar um cabeçalho `CQ-Action-Scope: ResourceOnly` adicional. Essa funcionalidade pode ser usada para liberar recursos específicos. Tudo isso sem invalidar outras partes do cache, como dados JSON. Esses dados são criados dinamicamente e exigem limpeza regular independente do cache. Por exemplo, representar dados obtidos de um sistema de terceiros para exibir notícias, cotações da bolsa e assim por diante.

### Exclusão e rearmazenamento de arquivos em cache {#delete-and-recache-files}

Emita uma solicitação HTTP que faça com que o Dispatcher exclua arquivos em cache e, imediatamente, recupere e armazene o arquivo em cache novamente. Exclua e imediatamente armazene arquivos em cache novamente quando houver probabilidade de os sites receberem solicitações simultâneas do cliente para a mesma página. O rearmazenamento em cache imediato garante que o Dispatcher recupere e armazene em cache a página apenas uma vez, e não uma vez para cada uma das solicitações simultâneas do cliente.

**Observação:** a exclusão e o rearmazenamento em cache de arquivos devem ser executados somente na instância de publicação. Quando executados da instância do autor, as condições de corrida ocorrem mediante tentativas de recuperar recursos antes de serem publicadas.

A solicitação HTTP tem o seguinte formato:

```xml
POST /dispatcher/invalidate.cache HTTP/1.1  
CQ-Action: Activate   
`Content-Type: text/plain  
CQ-Handle: path-pattern  
Content-Length: numchars in bodypage_path0

page_path1 
...  
page_pathn
```

Os caminhos das páginas para armazenar novamente em cache imediatamente são listados em linhas separadas no corpo da mensagem. O valor de `CQ-Handle` é o caminho de uma página que invalida as páginas para o rearmazenamento em cache. (Consulte o parâmetro `/statfileslevel` do item de configuração [Cache](dispatcher-configuration.md#main-pars_146_44_0010).) O exemplo de mensagem de solicitação HTTP a seguir exclui e torna a armazenar em cache o `/content/geometrixx-outdoors/en.html page`:

```xml
POST /dispatcher/invalidate.cache HTTP/1.1  
CQ-Action: Activate  
Content-Type: text/plain   
CQ-Handle: /content/geometrixx-outdoors/en/men.html  
Content-Length: 36

/content/geometrixx-outdoors/en.html
```

### Exemplo de servlet de liberação {#example-flush-servlet}

O código a seguir implementa um servlet que envia uma solicitação de invalidação para o Dispatcher. O servlet recebe uma mensagem de solicitação que contém os parâmetros `handle` e `page`. Esses parâmetros fornecem o valor do cabeçalho `CQ-Handle` e o caminho da página para refazer o armazenamento em cache, respectivamente. O servlet usa os valores para criar a solicitação HTTP para o Dispatcher.

Quando o servlet é implantado na instância de publicação, o seguinte URL faz com que o Dispatcher exclua a página /content/geometrixx-outdoors/en.html e, em seguida, armazene em cache uma nova cópia.

`10.36.79.223:4503/bin/flushcache/html?page=/content/geometrixx-outdoors/en.html&handle=/content/geometrixx-outdoors/en/men.html`

>[!NOTE]
>
>Este servlet de exemplo não é seguro e demonstra apenas o uso da mensagem de Solicitação HTTP POST. Sua solução deve proteger o acesso ao servlet.
>

```java
package com.adobe.example;

import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Service;
import org.apache.felix.scr.annotations.Property;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import org.apache.commons.httpclient.*;
import org.apache.commons.httpclient.methods.PostMethod;
import org.apache.commons.httpclient.methods.StringRequestEntity;

@Component(metatype=true)
@Service
public class Flushcache extends SlingSafeMethodsServlet {

 @Property(value="/bin/flushcache")
 static final String SERVLET_PATH="sling.servlet.paths";

 private Logger logger = LoggerFactory.getLogger(this.getClass());

 public void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  try{ 
      //retrieve the request parameters
      String handle = request.getParameter("handle");
      String page = request.getParameter("page");

      //hard-coding connection properties is a bad practice, but is done here to simplify the example
      String server = "localhost"; 
      String uri = "/dispatcher/invalidate.cache";

      HttpClient client = new HttpClient();

      PostMethod post = new PostMethod("https://"+host+uri);
      post.setRequestHeader("CQ-Action", "Activate");
      post.setRequestHeader("CQ-Handle",handle);
   
      StringRequestEntity body = new StringRequestEntity(page,null,null);
      post.setRequestEntity(body);
      post.setRequestHeader("Content-length", String.valueOf(body.getContentLength()));
      client.executeMethod(post);
      post.releaseConnection();
      //log the results
      logger.info("result: " + post.getResponseBodyAsString());
      }
  }catch(Exception e){
      logger.error("Flushcache servlet exception: " + e.getMessage());
  }
 }
}
```
