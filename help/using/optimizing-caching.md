---
title: Otimizar um site para desempenho do cache
description: Saiba como projetar seu site para potencializar os benefícios do armazenamento em cache.
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
redirecttarget: https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/configuring-performance.html
index: y
internal: n
snippet: y
source-git-commit: c41b4026a64f9c90318e12de5397eb4c116056d9
workflow-type: ht
source-wordcount: '1128'
ht-degree: 100%

---


# Otimizar um site para desempenho do cache {#optimizing-a-website-for-cache-performance}

<!-- 

Comment Type: remark
Last Modified By: Silviu Raiman (raiman)
Last Modified Date: 2017-10-25T04:13:34.919-0400

<p>This is a redirect to /experience-manager/6-2/sites/deploying/using/configuring-performance.html</p>

 -->

>[!NOTE]
>
>As versões do Dispatcher são independentes do AEM. Você pode ter sido redirecionado para esta página se clicou em um link para a documentação do Dispatcher. Esse link foi incorporado na documentação de uma versão anterior do AEM.

O Dispatcher oferece vários mecanismos integrados para otimizar o desempenho. Esta seção informa como projetar seu site para potencializar os benefícios do armazenamento em cache.

>[!NOTE]
>
>Pode ser útil ter em mente que o Dispatcher armazena o cache em um servidor da Web padrão. Isso significa que você:
>
>* pode armazenar em cache tudo o que se pode armazenar como uma página e solicitar usando um URL
>* não pode armazenar outras coisas, como cabeçalhos HTTP, cookies, dados de sessão e dados de formulário.
>
>Em geral, muitas estratégias de armazenamento em cache envolvem selecionar bons URLs e não depender desses dados adicionais.

## Usar codificação de página consistente {#using-consistent-page-encoding}

Os cabeçalhos de solicitação HTTP não são armazenados em cache e, portanto, poderão ocorrer problemas se você armazenar informações de codificação de página no cabeçalho. Nessa situação, quando o Dispatcher fornece uma página do cache, a codificação padrão do servidor Web é usada para a página. Há duas maneiras de evitar esse problema:

* Se você usar apenas uma codificação, verifique se a codificação usada no servidor Web é igual à codificação padrão do site do AEM.
* Para definir a codificação, use uma tag `<META>` na seção `head` do HTML, como no exemplo a seguir:

```xml
        <META http-equiv="Content-Type" content="text/html; charset=EUC-JP">
```

## Evitar parâmetros de URL {#avoid-url-parameters}

Se possível, evite parâmetros de URL para páginas que você deseja armazenar em cache. Por exemplo, se você tiver uma galeria de imagens, o seguinte URL nunca será armazenado em cache (a menos que o Dispatcher esteja [configurado adequadamente](dispatcher-configuration.md#main-pars_title_24)):

```xml
www.myCompany.com/pictures/gallery.html?event=christmas&amp;page=1
```

No entanto, você pode colocar esses parâmetros no URL da página, da seguinte maneira:

```xml
www.myCompany.com/pictures/gallery.christmas.1.html
```

>[!NOTE]
>
>Esse URL chama a mesma página e o mesmo modelo de gallery.html. Na definição do modelo, é possível especificar qual script renderiza a página ou usar o mesmo script para todas as páginas.

## Personalizar por URL {#customize-by-url}

Se você permitir que os usuários alterem o tamanho da fonte (ou qualquer outra personalização de layout), verifique se as diferentes personalizações são refletidas no URL.

Por exemplo, os cookies não são armazenados em cache. Dessa forma, se você armazenar o tamanho da fonte em um cookie (ou mecanismo semelhante), o tamanho da fonte não será preservado para a página em cache. Como resultado, o Dispatcher retorna documentos de qualquer tamanho de fonte aleatoriamente.

Incluir o tamanho da fonte no URL como um seletor evita esse problema:

```xml
www.myCompany.com/news/main.large.html
```

>[!NOTE]
>
>Para a maioria dos aspectos de layout, também é possível usar folhas de estilos e/ou scripts do lado do cliente. Um ou ambos funcionam bem com o armazenamento em cache.
>
>Este método também é útil para uma versão impressa, na qual você pode usar um URL como:
>
>`www.myCompany.com/news/main.print.html`
>
>Usando o recurso de curinga de script da definição do modelo, você pode especificar um script separado que renderize as páginas de impressão.

## Invalidar arquivos de imagem usados como títulos {#invalidating-image-files-used-as-titles}

Se você renderizou títulos de páginas ou outros textos como imagens, armazene os arquivos para que sejam excluídos após uma atualização de conteúdo na página:

1. Coloque o arquivo de imagem na mesma pasta da página.
1. Use o seguinte formato de nomenclatura para o arquivo de imagem:

   `<page file name>.<image file name>`

Por exemplo, você pode armazenar o título da página myPage.html no arquivo myPage.title.gif. Esse arquivo será automaticamente excluído se a página for atualizada, de modo que qualquer alteração no título da página será refletida automaticamente no cache.

>[!NOTE]
>
>O arquivo de imagem não existe necessariamente de forma física na instância do AEM. Você pode usar um script que cria dinamicamente o arquivo de imagem. O Dispatcher armazena o arquivo no servidor Web.

## Invalidar arquivos de imagem usados para navegação {#invalidating-image-files-used-for-navigation}

Se você usa imagens para as entradas de navegação, o método é basicamente o mesmo para títulos, apenas um pouco mais complexo. Armazene todas as imagens de navegação com as páginas de destino. Se você usar duas imagens para o normal e o ativo, poderá usar os seguintes scripts:

* Um script que exibe a página, como de costume.
* Um script que processa solicitações `.normal` e retorna a imagem normal.
* Um script que processa solicitações `.active` e retorna a imagem ativada.

É importante criar essas imagens com o mesmo identificador de nome da página para garantir que uma atualização de conteúdo exclua essas imagens, bem como a página.

Para páginas que não são modificadas, as imagens ainda permanecem no cache, embora as próprias páginas sejam invalidadas automaticamente.

## Personalização {#personalization}

O Dispatcher não pode armazenar dados personalizados em cache, portanto, é recomendável limitar a personalização ao local necessário. Para ilustrar o motivo:

* Se você usar uma página inicial personalizável livremente, essa página deverá ser composta sempre que um usuário a solicitar.
* Se, por outro lado, você oferecer a opção de dez páginas iniciais diferentes, poderá armazenar em cache cada uma delas para melhorar o desempenho.

>[!NOTE]
>
>Se você personalizar cada página (por exemplo, colocando o nome do usuário na barra de título), não poderá armazená-la em cache, o que pode causar um grande impacto no desempenho.
>
>No entanto, se necessário, você pode fazer o seguinte:
>
>* usar iFrames para dividir a página em uma parte que seja a mesma para todos os usuários e uma parte que seja a mesma para todas as páginas do usuário. Depois, é possível armazenar em cache essas duas partes.
>* usar o JavaScript do lado do cliente para exibir informações personalizadas. No entanto, é necessário assegurar que a página ainda seja exibida corretamente se um usuário desativar o JavaScript.
>

## Conexões adesivas {#sticky-connections}

As [conexões adesivas](dispatcher.md#TheBenefitsofLoadBalancing) garantem que os documentos de um usuário sejam todos compostos no mesmo servidor. Se um usuário sair dessa pasta e posteriormente retornar a ela, a conexão ainda permanecerá. Defina uma pasta para conter todos os documentos que exigem conexões fixas para o site. Tente não manter outros documentos nela. Isso impactará o balanceamento de carga se você usar páginas personalizadas e dados de sessão.

## Tipos de MIME {#mime-types}

Há duas maneiras pelas quais um navegador pode determinar o tipo de arquivo:

1. Pela sua extensão (por exemplo, .html, .gif e .jpg)
1. Pelo tipo MIME que o servidor envia com o arquivo.

Para a maioria dos arquivos, o tipo MIME está implícito na extensão de arquivo:

1. Pela sua extensão (por exemplo, .html, .gif e .jpg)
1. Pelo tipo MIME que o servidor envia com o arquivo.

Se o nome do arquivo não tiver extensão, ele será exibido como texto sem formatação.

O tipo MIME faz parte do cabeçalho HTTP e, como tal, o Dispatcher não o armazena em cache. O aplicativo AEM pode retornar arquivos que não tenham uma extensão de arquivo reconhecida. Se, em vez disso, os arquivos dependerem do tipo MIME, esses arquivos poderão ser exibidos incorretamente.

Para garantir que os arquivos sejam armazenados em cache corretamente, siga estas diretrizes:

* Certifique-se de que os arquivos sempre tenham a extensão adequada.
* Evite scripts de servidor de arquivos genéricos, que tenham URLs como download.jsp?file=2214. Reescreva o script para utilizar URLs que contenham a especificação do arquivo. Para o exemplo anterior, seria `download.2214.pdf`.

