---
source-git-commit: 0a1aa854ea286a30c3527be8fc7c0998726a663f
workflow-type: tm+mt
source-wordcount: '714'
ht-degree: 69%

---
# Diretrizes para colaboração na documentação do Adobe Experience Manager

## Filosofia da documentação

Os usuários do Adobe Experience Manager trabalham em ambientes extremamente competitivos, esforçando-se para criar experiências digitais para se destacar de seus concorrentes. Portanto, quando o Adobe fornece ferramentas avançadas no AEM, essas ferramentas são complementadas com documentação precisa e transparente. Ele permite que os clientes usem imediatamente seu investimento em AEM e maximizem seu retorno sobre o investimento.

O objetivo é disponibilizar a documentação do AEM aos usuários o quanto antes. Portanto, o Adobe prioriza uma documentação precisa e utilizável, e se esforça para atualizá-la e aprimorá-la continuamente.

## Contribuições à documentação

Para melhorar continuamente a documentação do AEM, toda a comunidade de usuários do AEM é bem-vinda para contribuir em sua elaboração. Seja por meio de solicitações de pull ou tópicos, as melhorias na documentação envolvem correções, esclarecimentos, expansões, entre outros exemplos.

## Padrões de documentação

Embora o Adobe receba contribuições para sua documentação, qualquer contribuição para a documentação do AEM em um pull request ou um problema deve estar em conformidade com os padrões de contribuição e documentação do Adobe.

As contribuições que não cumprirem esses padrões poderão ser rejeitadas.

### Os casos de uso padrão estão documentados no Adobe.

A documentação do AEM abrange casos de uso padrão. Casos de uso que não se enquadrarem no escopo de instalação e de uso padrão do produto não farão parte da documentação do AEM.

### A Adobe geralmente não documenta erros e suas soluções.

A documentação do AEM abrange casos de uso padrão. Por essa razão, os bugs, seus efeitos e soluções alternativas não são documentados.

As exceções a essa regra aplicam-se às notas de versão, nas quais problemas conhecidos podem ser listados com possíveis soluções aprovadas pelo Gerenciamento de produtos do.

### As contribuições à documentação não se destinam a responder perguntas técnicas.

Quaisquer ideias para melhorar a documentação do AEM são bem-vindas como contribuições. No entanto, comentários, tópicos e solicitações de pull devem ser realizados apenas como *contribuições*. Eles não devem envolver perguntas sobre como usar o AEM, implementar o projeto do AEM ou resolver problemas técnicos.

Você pode relatar qualquer dúvida sobre o uso do AEM ou erros técnicos. Use o processo normal de suporte por meio do [Portal de suporte corporativo Experience Cloud](https://experienceleague.adobe.com/pt-br?support-solution=General#support) ou discutido na [comunidade do Experience Manager](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/ct-p/adobe-experience-manager-community?profile.language=pt).

***As contribuições para a documentação do AEM não substituem o atendimento ao cliente da Adobe***, e quaisquer contribuições que busquem respostas a perguntas relacionadas ao suporte serão rejeitadas.

### As contribuições devem mencionar claramente as páginas pertinentes à documentação.

Se você criar um problema para sugerir melhorias na documentação, deverá incluir links para as páginas afetadas. Caso crie um tópico por meio de um link **Editar esta página** na documentação, o tópico será criado automaticamente com um link para a página.

Esse processo não se aplica a pull requests, que já fazem referência às páginas afetadas.

## Diretrizes de documentação

Solicitamos que qualquer contribuição para a documentação siga certas diretrizes de estilo.

Seguir essas diretrizes facilita a revisão da sua contribuição, o que agiliza a integração à documentação da Adobe.

### Idioma e estilo

#### Idioma

* A documentação do AEM foi criada e mantida em inglês americano.
* Mantenha as sentenças o mais simples possível.
* Use linguagem clara e concisa.

Observe que os(as) leitores(as) da documentação do AEM estão espalhados pelo mundo, e não se espera que sejam falantes nativos ou fluentes em inglês. Evite linguagem coloquial e utilize um vocabulário claro e simples.

#### Siga o Manual de estilo da Microsoft®

[O Manual de Estilo da Microsoft®](https://learn.microsoft.com/pt-br/style-guide/welcome/) é um guia de estilo gratuito que se concentra na documentação de softwares, e a documentação do AEM o segue sempre que possível.

### Formatação

| Item | Estilo |
|---|---|
| Elemento ou opção da interface do usuário | **negrito** |
| Nome do arquivo, caminho, entrada do usuário, valores de parâmetro | `monospaced` |
| Código, linha de comando | ```Code Block``` |

### Capturas de tela

As capturas de tela devem ser utilizadas com critério e somente quando uma descrição textual for insuficiente.

Não utilize marcadores ou outras anotações em capturas de tela (como quadros vermelhos, setas ou texto). Dessa forma, as capturas de tela são mais facilmente reutilizadas ou replicadas em versões localizadas da documentação.

### Referências específicas à versão

Evite referências diretas a uma versão específica em todo o conteúdo da documentação, sempre que possível. Esta recomendação torna a documentação mais flexível e extensível para versões futuras.

### Uso do Day, AEM, CQ, CRX

Em um artigo, sempre use o nome completo do produto **Adobe Experience Manager** na primeira vez em que for utilizado. Posteriormente, pode ser referido como **AEM**.

Não utilize Day, Day Software, CQ e CRX, exceto quando isso for inevitável, como em nomes de classe ou em referência ao histórico do AEM.


